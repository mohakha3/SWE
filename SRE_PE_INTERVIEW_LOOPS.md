SRE/PE Interview loops:


INITIAL SCREENING:


The initial screening process at Big Tech companies like Meta or Google is highly structured and designed to filter out candidates before they ever meet the full interview panel [sre.google]. 

For a specialized infrastructure role like Network Production Engineer or Network SRE, you will pass through a two-step screening pipeline. Because of your deep Service Provider (SP) background, your path through this pipeline is highly predictable. 

Here is exactly how they screen you initially, step-by-step: 




Step 1: The Automated Recruiter Resume Review
Before a technical engineer ever sees your name, a recruiter or an automated tracking system (ATS) filters your resume. 

What they look for: They scan your profile to ensure you are a Systems/Infrastructure Specialist, not just a legacy configuration tech.
How you clear it: You pass this effortlessly by ensuring your resume uses system-centric keywords. Instead of saying "configured Cisco routers," your profile must highlight keywords like BGP Scaling, EVPN overlays, Linux, Automation, and Telemetry (gRPC/gNMI). This instantly routes your profile to the specialized Infrastructure team rather than the general web developer bucket. 



Step 2: The Technical Screening Call (The Guardrail)
Once the recruiter clears your resume, you will schedule a 45-to-60 minute phone/video call with a single engineer from the team. This is a "pass/fail" guardrail panel [sre.google]. They are checking to see if you possess the baseline survival skills required to handle the full loop. 

This screening call is split into two distinct halves: 


                  ┌─────────────────────────────────────────┐
                  │      45-MINUTE TECHNICAL SCREENING      │
                  └────────────────────┬────────────────────┘
                                       │
         ┌─────────────────────────────┴─────────────────────────────┐
         ▼                                                           ▼
┌─────────────────────────┐                                 ┌─────────────────────────┐
│  Part A: Linux & Network │                                 │  Part B: Coding Test    │
│      (20 Minutes)       │                                 │      (25 Minutes)       │
├─────────────────────────┤                                 ├─────────────────────────┤
│ Rapid-fire verbal Q&A   │                                 │ Writing a simple,       │
│ on Linux commands, file │                                 │ live script in a blank  │
│ paths, and standard BGP │                                 │ editor (like CoderPad)  │
│ protocol mechanics.     │                                 │ to process basic text.  │
└─────────────────────────┘                                 └─────────────────────────┘

What Part A (Verbal Q&A) Looks Like: 

The engineer will fire rapid questions to test your intuition. Because you are studying Brian Ward's How Linux Works and Richard Blum's Shell Scripting Bible in Phase 1, you will answer these easily: 

"What file under the /proc filesystem would you read to check memory utilization?" (Answer: /proc/meminfo)
"How do you find which process is listening on port 80 inside the shell?" (Answer: netstat -tulpn or ss -tulpn)
"Explain the exact difference between a TCP handshake and a UDP datagram packet stream." (Your SP background makes this trivial). 

What Part B (The Coding Test) Looks Like: 

You will open a shared, non-assisted text window (like CoderPad). The question will be an uncomplicated, entry-level script problem—much easier than the full loop panel. 

Typical Screen Prompt: "Write a short script that opens a raw text file containing a list of IP addresses, filters out any invalid entries, and returns a count of how many times each valid IP appears." 
Why typing in nano now saves you: The interviewer wants to see that you don't freeze up on basic syntax. If you can confidently declare a loop, open a text file, and handle a simple map index in Go without leaning on an IDE, you pass. 



The Screening Scorecard Outcome
The screening engineer only has to answer one question for the hiring committee: "Does this candidate have a high probability of passing the full panel?" 

If you ace the networking/systems questions and write a functional, error-handled script to solve their baseline problem, you get a "Strong Pass" and immediately advance to the full loop (the Code, OS Internals, and System Design panels we mapped out previously).

========

THE FULL 4-PANEL INTERVIEW STRUCTURE:

Panel Type	Focus & Time Split	What You Do	Your Curriculum Weapon
1. Systems Coding	• 45 minutes total
• 30 mins coding
• 15 mins complexity	Write a 20-to-50 line script in Go or Python to parse logs, manipulate data arrays, or handle basic text streams.	Donovan (Go Syntax) & Wiener (DSA)
2. Linux OS Internals	• 45 minutes total
• Pure interactive Q&A	Answer deep architectural questions about how the OS schedules threads, handles virtual memory, and manages resources.	Ward (Linux) & Kerrisk (Syscalls)
3. System Design	• 45 minutes total
• Pure architectural whiteboard	Design a massive distributed infrastructure system (e.g., Design a configuration deployment pipeline for 100,000 servers).	Kleppmann (DDIA) & Jeffery (Distributed Go)
4. Behavioral	• 45 minutes total
• Culture fit / Leadership	Discuss how you handle conflict, navigate production outages, manage project deadlines, and mentor junior engineers.	Your 20+ years of SP experience


Breakdown of the 3 Technical Panels

Part 1: Systems Coding (The Syntax Test) 

The Vibe: You are given an empty, plain text screen (like coderpad) with an interviewer watching you type. 
The Goal: They want to see if you can translate logical thoughts into bug-free code under time pressure. They test basic data structures: string parsing, arrays/slices, hash maps, and loops. 
The Bar: You do not need to solve hyper-academic, ultra-complex algorithms. They just need to see that you can write clean code that handles errors gracefully and runs efficiently. 

Part 2: Linux OS Internals (The Context Test) 

The Vibe: There is no coding in this round. It is a rapid-fire, highly interactive technical conversation. 
The Goal: They want to see if you understand the underlying machinery of the operating system. They will push you until you hit your breaking point. They will ask questions like: "What is the difference between a process and a thread?", "What happens at the kernel layer when a hardware interrupt fires?", or "Explain the exact steps the OS takes when a packet hits the ring buffer of a NIC." 
The Bar: This is where traditional network engineers usually fail because they treat the server like a black box. Ward's How Linux Works is engineered specifically to make you pass this round. 

Part 3: Systems Design (The Architecture Test) 

The Vibe: You are looking at a digital whiteboard. You are the lead architect, and the interviewer acts as a peer reviewing your blueprint. 
The Goal: They want to see how you think at scale. They will give you a vague, massive prompt: "Design a global real-time telemetry collector for our edge switches." You have to ask clarifying questions, define the API boundaries, choose the database tier (SQL vs. NoSQL), handle network latency, and design for high availability so the system survives a fiber cut. 
The Bar: This is where your maturity and age shine. You already know how to design large architectures. By overlaying Martin Kleppmann's Designing Data-Intensive Applications on top of your existing design intuition, you will easily secure an elite score on this panel. 




