Go Scripts:

Go does not use traditional classes or inheritance. Instead, it relies on independent structs(perfect for fixed network packets) and implicit interfaces. This allows you to decouple vendor logic entirely. Separate drivers for Cisco and Arista can satisfy a generic Device interface without any strict structural dependency on each other.


Note: On mac terminal, one cannot create a write directory folder in root, it will be readonly. Instead create one folder under 


** My first go lang automation script to login into cisco devices and collect show logs **:

1. First, install go on macbook:

brew install go

2. Then create a directory from where go script will be running and saving your files. In my example, main directory name is GO_MAIN and sub-directory is cisco-5501
zsh

mkdir GO_MAIN/cisco-5501 && cd GO_MAIN/cisco-5501

go mod init cisco-5501

go get golang.org/x/crypto/ssh

3. Code to login into the device/devices , all at the same time and take the output from them: 

cat << 'EOF' > main.go
package main

import (
	"bytes"
	"fmt"
	"log"
	"os"
	"sync"
	"time"

	"golang.org/x/crypto/ssh"
)

type Router struct {
	IP       string
	Username string
	Password string
}

func main() {
	commands := []string{
		"terminal length 0",
		"show version",
		"show install active summary",
		"show install inactive summary",
                "show install repository",
                "show install committed",
		"exit",
	}

	routers := []Router{
		{IP: "172.18.104.12:22", Username: “xxxx”, Password: “xxxx”},
		{IP: "172.18.87.83:22", Username: “xxxx”, Password: “xxxx”},
		{IP: "172.18.120.232:22", Username: “xxxx”, Password: “xxxx”},
	}

	var wg sync.WaitGroup

	for _, r := range routers {
		wg.Add(1)
		go func(router Router) {
			defer wg.Done()
			collectRouterLogs(router, commands)
		}(r)
	}

	wg.Wait()
	fmt.Println("All router tasks completed.")
}

func collectRouterLogs(router Router, commands []string) {
	fmt.Printf("[%s] Connecting...\n", router.IP)

	config := &ssh.ClientConfig{
		User: router.Username,
		Auth: []ssh.AuthMethod{
			ssh.Password(router.Password),
		},
		HostKeyCallback: ssh.InsecureIgnoreHostKey(),
		Timeout:         10 * time.Second,
	}

	client, err := ssh.Dial("tcp", router.IP, config)
	if err != nil {
		log.Printf("[%s] Connection error: %v\n", router.IP, err)
		return
	}
	defer client.Close()

	session, err := client.NewSession()
	if err != nil {
		log.Printf("[%s] Session error: %v\n", router.IP, err)
		return
	}
	defer session.Close()

	modes := ssh.TerminalModes{
		ssh.ECHO:          0,
		ssh.TTY_OP_ISPEED: 14400,
		ssh.TTY_OP_OSPEED: 14400,
	}
	if err := session.RequestPty("vt100", 80, 40, modes); err != nil {
		log.Printf("[%s] PTY error: %v\n", router.IP, err)
		return
	}

	stdin, err := session.StdinPipe()
	if err != nil {
		log.Printf("[%s] Stdin pipe error: %v\n", router.IP, err)
		return
	}

	var stdoutBuffer bytes.Buffer
	session.Stdout = &stdoutBuffer

	if err := session.Shell(); err != nil {
		log.Printf("[%s] Shell error: %v\n", router.IP, err)
		return
	}

	for _, cmd := range commands {
		fmt.Fprintln(stdin, cmd)
	}

	session.Wait()

	cleanIP := bytes.ReplaceAll([]byte(router.IP), []byte(":"), []byte("_"))
	outputFile := fmt.Sprintf("router_%s.log", cleanIP)

	err = os.WriteFile(outputFile, stdoutBuffer.Bytes(), 0644)
	if err != nil {
		log.Printf("[%s] File write error: %v\n", router.IP, err)
		return
	}

	fmt.Printf("[%s] Saved output to %s\n", router.IP, outputFile)
}
EOF


nano main.go

sed -i '' 's/[“”]/"/g' main.go

go mod tidy

go run main.go

cat cisco_router.log


if you face any of these issues:

# command-line-arguments
./main.go:19:14: invalid character U+201C '“' in identifier
./main.go:19:25: invalid character U+201D '”' in identifier


Execute: sed -i '' 's/[“”]/"/g' main.go



# command-line-arguments
./main.go:24:18: syntax error: unexpected name active in composite literal; possibly missing comma or }

check for missing commas after the "command" list.





** go lang automation script to login into cisco devices, collect single show tech [xr vm], sftp from the device**:

Since we need to wait for show tech to collect completely, we poll the router to ensure that size of the show tech collected stops growing in size

cat << 'EOF' > main.go
package main

import (
	"fmt"
	"io"
	"log"
	"os"
	"path/filepath"
	"strings"
	"sync"
	"time"

	"github.com/pkg/sftp"
	"golang.org/x/crypto/ssh"
)

type Router struct {
	IP       string
	Username string
	Password string
}

func main() {
	routers := []Router{
		{IP: "172.18.120.232:22", Username: "xxx", Password: "xxxx"},
		}

	macOutputDir := "./router_downloads"
	if err := os.MkdirAll(macOutputDir, 0755); err != nil {
		log.Fatalf("Failed to create Mac output directory: %v", err)
	}

	var wg sync.WaitGroup

	for _, r := range routers {
		wg.Add(1)
		go func(router Router) {
			defer wg.Done()
			downloadShowTech(router, macOutputDir)
		}(r)
	}

	wg.Wait()
	fmt.Println("All tech-support collection tasks completed.")
}

func downloadShowTech(router Router, macOutputDir string) {
	fmt.Printf("[%s] Connecting via SSH...\n", router.IP)

	config := &ssh.ClientConfig{
		User: router.Username,
		Auth: []ssh.AuthMethod{
			ssh.Password(router.Password),
		},
		HostKeyCallback: ssh.InsecureIgnoreHostKey(),
		Timeout:         30 * time.Second,
	}

	client, err := ssh.Dial("tcp", router.IP, config)
	if err != nil {
		log.Printf("[%s] Connection error: %v\n", router.IP, err)
		return
	}
	defer client.Close()

	session, err := client.NewSession()
	if err != nil {
		log.Printf("[%s] SSH Session error: %v\n", router.IP, err)
		return
	}

	modes := ssh.TerminalModes{
		ssh.ECHO:          0,
		ssh.TTY_OP_ISPEED: 14400,
		ssh.TTY_OP_OSPEED: 14400,
	}
	if err := session.RequestPty("vt100", 80, 40, modes); err != nil {
		log.Printf("[%s] PTY error: %v\n", router.IP, err)
		session.Close()
		return
	}

	stdin, err := session.StdinPipe()
	if err != nil {
		log.Printf("[%s] Stdin pipe error: %v\n", router.IP, err)
		session.Close()
		return
	}

	if err := session.Shell(); err != nil {
		log.Printf("[%s] Shell error: %v\n", router.IP, err)
		session.Close()
		return
	}

	fileName := "5508.tgz"
	remoteSFTPPath := "/harddisk:/5508.tgz"

	fmt.Printf("[%s] Executing background show tech command...\n", router.IP)

	fmt.Fprintln(stdin, "terminal length 0")
	fmt.Fprintln(stdin, "show tech-support file harddisk:/5508 background")

	time.Sleep(1 * time.Second)

	sftpClient, err := sftp.NewClient(client)
	if err != nil {
		log.Printf("[%s] SFTP connection failed: %v\n", router.IP, err)
		session.Close()
		return
	}
	defer sftpClient.Close()

	var previousSize int64 = -1
	stableChecks := 0

	fmt.Printf("[%s] Polling harddisk every 2s for %s...\n", router.IP, fileName)

	for i := 0; i < 90; i++ {
		time.Sleep(2 * time.Second)

		stat, err := sftpClient.Stat(remoteSFTPPath)
		if err != nil {
			continue
		}

		currentSize := stat.Size()
		fmt.Printf("[%s] %s size: %d bytes\n", router.IP, fileName, currentSize)

		if currentSize > 0 && currentSize == previousSize {
			stableChecks++
			if stableChecks >= 2 {
				fmt.Printf("[%s] Show tech generation finished!\n", router.IP)
				break
			}
		} else {
			stableChecks = 0
		}
		previousSize = currentSize
	}

	fmt.Fprintln(stdin, "exit")
	session.Close()

	fmt.Printf("[%s] Downloading %s to Mac...\n", router.IP, fileName)

	remoteFile, err := sftpClient.Open(remoteSFTPPath)
	if err != nil {
		log.Printf("[%s] Failed to open %s on harddisk: %v\n", router.IP, remoteSFTPPath, err)
		return
	}
	defer remoteFile.Close()

	cleanIP := strings.ReplaceAll(router.IP, ":", "_")
	localFilePath := filepath.Join(macOutputDir, fmt.Sprintf("showtech_%s_%s", cleanIP, fileName))

	localFile, err := os.Create(localFilePath)
	if err != nil {
		log.Printf("[%s] Failed to create local file on Mac: %v\n", router.IP, err)
		return
	}
	defer localFile.Close()

	bytesCopied, err := io.Copy(localFile, remoteFile)
	if err != nil {
		log.Printf("[%s] Download error: %v\n", router.IP, err)
		return
	}

	fmt.Printf("[%s] Successfully downloaded %d bytes to %s\n", router.IP, bytesCopied, localFilePath)
}
EOF


nano main.go

sed -i '' 's/[“”]/"/g' main.go

go mod tidy

go run main.go



** go lang automation script to login into cisco devices, collect MULTIPLE show tech [xr vm], sftp from the device**:


cat << 'EOF' > main.go
package main

import (
	"fmt"
	"io"
	"log"
	"os"
	"path/filepath"
	"strings"
	"sync"
	"time"

	"github.com/pkg/sftp"
	"golang.org/x/crypto/ssh"
)

type Router struct {
	IP       string
	Username string
	Password string
}

type ShowTechJob struct {
	Command  string
	FileName string
}

func main() {
	routers := []Router{
		{IP: "172.18.120.232:22", Username: "xxxx", Password: "xxxx"},
	}

	macOutputDir := "./router_downloads"
	if err := os.MkdirAll(macOutputDir, 0755); err != nil {
		log.Fatalf("Failed to create Mac output directory: %v", err)
	}

	var wg sync.WaitGroup

	for _, r := range routers {
		wg.Add(1)
		go func(router Router) {
			defer wg.Done()
			downloadShowTech(router, macOutputDir)
		}(r)
	}

	wg.Wait()
	fmt.Println("All tech-support collection tasks completed.")
}

func downloadShowTech(router Router, macOutputDir string) {
	fmt.Printf("[%s] Connecting via SSH...\n", router.IP)

	config := &ssh.ClientConfig{
		User: router.Username,
		Auth: []ssh.AuthMethod{
			ssh.Password(router.Password),
		},
		HostKeyCallback: ssh.InsecureIgnoreHostKey(),
		Timeout:         30 * time.Second,
	}

	client, err := ssh.Dial("tcp", router.IP, config)
	if err != nil {
		log.Printf("[%s] Connection error: %v\n", router.IP, err)
		return
	}
	defer client.Close()

	session, err := client.NewSession()
	if err != nil {
		log.Printf("[%s] SSH Session error: %v\n", router.IP, err)
		return
	}

	modes := ssh.TerminalModes{
		ssh.ECHO:          0,
		ssh.TTY_OP_ISPEED: 14400,
		ssh.TTY_OP_OSPEED: 14400,
	}
	if err := session.RequestPty("vt100", 80, 40, modes); err != nil {
		log.Printf("[%s] PTY error: %v\n", router.IP, err)
		session.Close()
		return
	}

	stdin, err := session.StdinPipe()
	if err != nil {
		log.Printf("[%s] Stdin pipe error: %v\n", router.IP, err)
		session.Close()
		return
	}

	if err := session.Shell(); err != nil {
		log.Printf("[%s] Shell error: %v\n", router.IP, err)
		session.Close()
		return
	}

	// 5 distinct show tech jobs mapping unique commands to filenames
	jobs := []ShowTechJob{
		{Command: "show tech-support file harddisk:/showtech1 background", FileName: "showtech1.tgz"},
		{Command: "show tech-support mpls ldp file harddisk:/showtech2 background", FileName: "showtech2.tgz"},
		{Command: "show tech-support fabric file harddisk:/showtech3 background", FileName: "showtech3.tgz"},
		{Command: "show tech-support system file harddisk:/showtech4 background", FileName: "showtech4.tgz"},
	}

	sftpClient, err := sftp.NewClient(client)
	if err != nil {
		log.Printf("[%s] SFTP connection failed: %v\n", router.IP, err)
		session.Close()
		return
	}
	defer sftpClient.Close()

	fmt.Fprintln(stdin, "terminal length 0")

	// Process each job sequentially
	for idx, job := range jobs {
		fmt.Printf("[%s] Executing Job %d/5: %s...\n", router.IP, idx+1, job.Command)
		fmt.Fprintln(stdin, job.Command)

		time.Sleep(1 * time.Second)

		remoteSFTPPath := fmt.Sprintf("/harddisk:/%s", job.FileName)
		var previousSize int64 = -1
		stableChecks := 0

		fmt.Printf("[%s] Polling harddisk every 2s for %s...\n", router.IP, job.FileName)

		for i := 0; i < 90; i++ {
			time.Sleep(2 * time.Second)

			stat, err := sftpClient.Stat(remoteSFTPPath)
			if err != nil {
				continue
			}

			currentSize := stat.Size()
			fmt.Printf("[%s] %s size: %d bytes\n", router.IP, job.FileName, currentSize)

			if currentSize > 0 && currentSize == previousSize {
				stableChecks++
				if stableChecks >= 2 {
					fmt.Printf("[%s] Show tech generation finished for %s!\n", router.IP, job.FileName)
					break
				}
			} else {
				stableChecks = 0
			}
			previousSize = currentSize
		}

		fmt.Printf("[%s] Downloading %s to Mac...\n", router.IP, job.FileName)

		remoteFile, err := sftpClient.Open(remoteSFTPPath)
		if err != nil {
			log.Printf("[%s] Failed to open %s on harddisk: %v\n", router.IP, remoteSFTPPath, err)
			continue
		}

		cleanIP := strings.ReplaceAll(router.IP, ":", "_")
		localFilePath := filepath.Join(macOutputDir, fmt.Sprintf("showtech_%s_%s", cleanIP, job.FileName))

		localFile, err := os.Create(localFilePath)
		if err != nil {
			log.Printf("[%s] Failed to create local file on Mac: %v\n", router.IP, err)
			remoteFile.Close()
			continue
		}

		bytesCopied, err := io.Copy(localFile, remoteFile)
		localFile.Close()
		remoteFile.Close()

		if err != nil {
			log.Printf("[%s] Download error for %s: %v\n", router.IP, job.FileName, err)
			continue
		}

		fmt.Printf("[%s] Successfully downloaded %d bytes to %s\n", router.IP, bytesCopied, localFilePath)
	}

	fmt.Fprintln(stdin, "exit")
	session.Close()
}
EOF

nano main.go

sed -i '' 's/[“”]/"/g' main.go

go mod tidy

go run main.go




** Go lang automation script to login into cisco devices, collect MULTIPLE show tech from xr vm and ADMIN vm, copy from admin vm to xr vm and then sftp from the device, in one single click **:



cat << 'EOF' > main.go
package main

import (
	"bytes"
	"fmt"
	"io"
	"log"
	"os"
	"path/filepath"
	"regexp"
	"strings"
	"sync"
	"time"

	"github.com/pkg/sftp"
	"golang.org/x/crypto/ssh"
)

type Router struct {
	IP       string
	Username string
	Password string
}

type ShowTechJob struct {
	Command  string
	FileName string
	IsAdmin  bool
}

func main() {
	routers := []Router{
		{IP: "x.x.x.x:22", Username: "xxxx", Password: "xxxxx"},
	}

	macOutputDir := "./router_downloads"
	if err := os.MkdirAll(macOutputDir, 0755); err != nil {
		log.Fatalf("Failed to create Mac output directory: %v", err)
	}

	var wg sync.WaitGroup

	for _, r := range routers {
		wg.Add(1)
		go func(router Router) {
			defer wg.Done()
			downloadShowTech(router, macOutputDir)
		}(r)
	}

	wg.Wait()
	fmt.Println("All tech-support collection tasks completed.")
}

func downloadShowTech(router Router, macOutputDir string) {
	fmt.Printf("[%s] Connecting via SSH...\n", router.IP)

	config := &ssh.ClientConfig{
		User: router.Username,
		Auth: []ssh.AuthMethod{
			ssh.Password(router.Password),
		},
		HostKeyCallback: ssh.InsecureIgnoreHostKey(),
		Timeout:         30 * time.Second,
	}

	client, err := ssh.Dial("tcp", router.IP, config)
	if err != nil {
		log.Printf("[%s] Connection error: %v\n", router.IP, err)
		return
	}
	defer client.Close()

	session, err := client.NewSession()
	if err != nil {
		log.Printf("[%s] SSH Session error: %v\n", router.IP, err)
		return
	}

	modes := ssh.TerminalModes{
		ssh.ECHO:          0,
		ssh.TTY_OP_ISPEED: 14400,
		ssh.TTY_OP_OSPEED: 14400,
	}

	if err := session.RequestPty("vt100", 500, 40, modes); err != nil {
		log.Printf("[%s] PTY error: %v\n", router.IP, err)
		session.Close()
		return
	}

	stdin, err := session.StdinPipe()
	if err != nil {
		log.Printf("[%s] Stdin pipe error: %v\n", router.IP, err)
		session.Close()
		return
	}

	var outBuf bytes.Buffer
	session.Stdout = &outBuf

	if err := session.Shell(); err != nil {
		log.Printf("[%s] Shell error: %v\n", router.IP, err)
		session.Close()
		return
	}

	sftpClient, err := sftp.NewClient(client)
	if err != nil {
		log.Printf("[%s] SFTP connection failed: %v\n", router.IP, err)
		session.Close()
		return
	}
	defer sftpClient.Close()

	fmt.Fprintln(stdin, "terminal length 0")
	fmt.Fprintln(stdin, "terminal width 500")

	// Detect active RP VM location (0/RP0/CPU0/VM1 or 0/RP1/CPU0/VM1)
	activeRPLocation := detectActiveRPLocation(stdin, &outBuf)
	fmt.Printf("[%s] Active RP VM Location: %s\n", router.IP, activeRPLocation)

	jobs := []ShowTechJob{
		{
			Command:  "show tech-support routing bgp file harddisk:/BGP_TECH background",
			FileName: "BGP_TECH.tgz",
			IsAdmin:  false,
		},
		{
			Command: "show tech-support ctrace",
			IsAdmin: true,
		},
	}

	for idx, job := range jobs {
		targetFileName := job.FileName

		if job.IsAdmin {
			fmt.Printf("[%s] Executing Admin Job %d/%d entirely on sysadmin side...\n", router.IP, idx+1, len(jobs))

			// Step 1: Transition into sysadmin CLI shell
			fmt.Printf("[%s] Entering admin mode...\n", router.IP)
			outBuf.Reset()
			fmt.Fprintln(stdin, "admin")
			waitForPrompt(stdin, &outBuf, 15*time.Second)

			// Step 2: Directory listing BEFORE running show tech on sysadmin
			outBuf.Reset()
			fmt.Fprintln(stdin, "dir harddisk:/showtech/")
			time.Sleep(3 * time.Second)
			beforeFiles := parseTechFileList(outBuf.String())

			// Step 3: Run show tech-support inside sysadmin
			fmt.Printf("[%s] Executing '%s' inside sysadmin...\n", router.IP, job.Command)
			outBuf.Reset()
			fmt.Fprintln(stdin, job.Command)

			// Wait for sysadmin CLI prompt (#) to return after completion
			waitForPrompt(stdin, &outBuf, 240*time.Second)

			// Step 4: Directory listing AFTER running show tech on sysadmin
			outBuf.Reset()
			fmt.Fprintln(stdin, "dir harddisk:/showtech/")
			time.Sleep(3 * time.Second)
			afterFiles := parseTechFileList(outBuf.String())

			// Step 5: Discover newly generated filename
			generatedFile := findNewFile(beforeFiles, afterFiles)
			if generatedFile == "" {
				log.Printf("[%s] Could not discover newly generated file in sysadmin harddisk:/showtech/\n", router.IP)
				fmt.Fprintln(stdin, "exit")
				continue
			}

			fmt.Printf("[%s] Discovered file on sysadmin: %s\n", router.IP, generatedFile)

			// Step 6: Copy file from sysadmin to XR VM using explicit 'location 0/RP0/CPU0/VM1' format
			copyCmd := fmt.Sprintf("copy harddisk:/showtech/%s harddisk:/%s location %s", generatedFile, generatedFile, activeRPLocation)
			fmt.Printf("[%s] Executing Copy on sysadmin: %s\n", router.IP, copyCmd)

			outBuf.Reset()
			fmt.Fprintln(stdin, copyCmd)
			time.Sleep(2 * time.Second)

			// Confirm default destination filename prompt if presented
			fmt.Fprintln(stdin, "")
			time.Sleep(2 * time.Second)

			fmt.Printf("[%s] Sysadmin Copy Output:\n%s\n", router.IP, outBuf.String())

			// Wait for copy operation to complete inside sysadmin prompt
			fmt.Printf("[%s] Waiting for sysadmin copy operation to complete...\n", router.IP)
			waitForPrompt(stdin, &outBuf, 90*time.Second)

			// Step 7: Exit sysadmin mode back to XR VM
			fmt.Printf("[%s] Exiting sysadmin mode...\n", router.IP)
			fmt.Fprintln(stdin, "exit")
			time.Sleep(2 * time.Second)
			waitForPrompt(stdin, &outBuf, 10*time.Second)

			targetFileName = generatedFile

		} else {
			// Standard XR VM job logic
			fmt.Printf("[%s] Executing XR Job %d/%d: %s...\n", router.IP, idx+1, len(jobs), job.Command)
			outBuf.Reset()
			fmt.Fprintln(stdin, job.Command)
			time.Sleep(1 * time.Second)
		}

		// SFTP Download Loop from XR harddisk:
		remoteSFTPPath := fmt.Sprintf("/harddisk:/%s", targetFileName)
		var previousSize int64 = -1
		stableChecks := 0

		fmt.Printf("[%s] Polling XR harddisk every 2s for %s...\n", router.IP, targetFileName)

		for i := 0; i < 90; i++ {
			time.Sleep(2 * time.Second)

			stat, err := sftpClient.Stat(remoteSFTPPath)
			if err != nil {
				continue
			}

			currentSize := stat.Size()
			fmt.Printf("[%s] %s size: %d bytes\n", router.IP, targetFileName, currentSize)

			if currentSize > 0 && currentSize == previousSize {
				stableChecks++
				if stableChecks >= 2 {
					fmt.Printf("[%s] File %s ready on XR harddisk!\n", router.IP, targetFileName)
					break
				}
			} else {
				stableChecks = 0
			}
			previousSize = currentSize
		}

		fmt.Printf("[%s] Downloading %s to local machine...\n", router.IP, targetFileName)

		remoteFile, err := sftpClient.Open(remoteSFTPPath)
		if err != nil {
			log.Printf("[%s] Failed to open %s on harddisk: %v\n", router.IP, remoteSFTPPath, err)
			continue
		}

		cleanIP := strings.ReplaceAll(router.IP, ":", "_")
		localFilePath := filepath.Join(macOutputDir, fmt.Sprintf("showtech_%s_%s", cleanIP, targetFileName))

		localFile, err := os.Create(localFilePath)
		if err != nil {
			log.Printf("[%s] Failed to create local file: %v\n", router.IP, err)
			remoteFile.Close()
			continue
		}

		bytesCopied, err := io.Copy(localFile, remoteFile)
		localFile.Close()
		remoteFile.Close()

		if err != nil {
			log.Printf("[%s] Download error for %s: %v\n", router.IP, targetFileName, err)
			continue
		}

		fmt.Printf("[%s] Successfully downloaded %d bytes to %s\n", router.IP, bytesCopied, localFilePath)
	}

	fmt.Fprintln(stdin, "exit")
	session.Close()
}

// Detects whether active RP VM is 0/RP0/CPU0/VM1 or 0/RP1/CPU0/VM1
func detectActiveRPLocation(stdin io.Writer, outBuf *bytes.Buffer) string {
	outBuf.Reset()
	fmt.Fprintln(stdin, "show redundancy summary")
	time.Sleep(2 * time.Second)

	output := outBuf.String()
	if strings.Contains(output, "0/RP1") {
		re := regexp.MustCompile(`0/RP1\s+.*Active`)
		if re.MatchString(output) {
			return "0/RP1/CPU0/VM1"
		}
	}
	return "0/RP0/CPU0/VM1"
}

// Parses directory output into a slice of filenames (.tgz or .tar.gz)
func parseTechFileList(dirOutput string) []string {
	re := regexp.MustCompile(`([a-zA-Z0-9_\-\.]+\.(tgz|tar\.gz))`)
	return re.FindAllString(dirOutput, -1)
}

// Compares "before" and "after" slices to find newly created file
func findNewFile(before, after []string) string {
	beforeMap := make(map[string]bool)
	for _, f := range before {
		beforeMap[f] = true
	}

	for _, f := range after {
		if !beforeMap[f] {
			return f
		}
	}

	if len(after) > 0 {
		return after[len(after)-1]
	}
	return ""
}

// Blocks until CLI prompt (# or >) reappears
func waitForPrompt(stdin io.Writer, outBuf *bytes.Buffer, timeout time.Duration) {
	start := time.Now()
	for time.Since(start) < timeout {
		time.Sleep(3 * time.Second)
		output := outBuf.String()
		trimmed := strings.TrimSpace(output)
		if strings.HasSuffix(trimmed, "#") || strings.HasSuffix(trimmed, ">") {
			break
		}
	}
}
EOF




nano main.go

sed -i '' 's/[“”]/"/g' main.go

go mod tidy

go run main.go




