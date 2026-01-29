I found these papers interesting and intend to read them soon 

 1. [**Smashing the Stack for Fun and Profit**](http://www.phrack.org/phrack/49/P49-14)
The foundational paper that started modern binary exploitation. Understanding this is essential for everything that follows.

2. [**Security Problems in the TCP/IP Protocol Suite**](https://www1.cs.columbia.edu/~smb/papers/ipext.pdf)
Still relevant after 30 years. Covers sequence number spoofing, routing attacks, and authentication issues that underpin modern network attacks.

3. [**The Geometry of Innocent Flesh on the Bone: Return-into-libc without Function Calls**](https://dl.acm.org/doi/10.1145/1315245.1315313)
Introduced ROP, the dominant code reuse attack technique that bypasses DEP/NX protections.

4. [**Bypassing non-executable-stack during exploitation using return-to-libc**](https://www.scribd.com/document/234644747/Return-to-Libc)
Shows how to exploit systems with NX/DEP protections by reusing existing libc functions instead of injecting shellcode.

5. [**On the Effectiveness of Address-Space Randomization**](https://dl.acm.org/doi/10.1145/1030083.1030124)
Analyzes ASLR effectiveness and limitations, demonstrating early techniques for bypassing randomization.

6. [**Rule-Based Static Analysis of Network Protocol Implementations**](https://www.usenix.org/legacy/events/sec06/tech/full_papers/udrea/udrea.pdf)
Introduces techniques for finding protocol implementation flaws, still used in modern analysis tools.

7. [**Control-Flow Integrity Principles, Implementations, and Applications**](https://users.soe.ucsc.edu/~abadi/Papers/cfi-tissec-revised.pdf)
The seminal paper on CFI, a defense mechanism that counters code reuse attacks by enforcing valid control flow graphs.

8. [**Just-In-Time Code Reuse: On the Effectiveness of Fine-Grained Address Space Layout Randomization**](https://www.ieee-security.org/TC/SP2013/papers/4977a574.pdf)
Analyzes why coarse-grained ASLR is insufficient and motivates modern fine-grained approaches using just-in-time gadget discovery.

9. [**Off-Path TCP Exploits: Global Rate Limit Considered Dangerous**](https://www.usenix.org/system/files/conference/usenixsecurity16/sec16_paper_cao.pdf)
Shows how off-path attackers can infer TCP sequence numbers and hijack connections—demonstrates modern TCP/IP weaknesses.

10. [**SoK: Eternal War in Memory**](https://nebelwelt.net/files/14SP.pdf)
Comprehensive survey of memory corruption attacks and defenses—essential for understanding the full exploitation landscape.

11. [**RADIUS/UDP Considered Harmful**](https://www.usenix.org/system/files/usenixsecurity24-goldberg.pdf)
Breaks a decades-old authentication protocol using MD5 collisions. Shows protocol-level attacks remain critical in modern infrastructure.

12. [**RE-Mind: A First Look Inside the Mind of a Reverse Engineer**](https://www.usenix.org/system/files/sec22-mantovani.pdf)
Uses cognitive analysis to understand how experts reverse engineer binaries—improves your own methodology.

13. [**MetaAware: Identifying Metamorphic Malware**](https://www.acsac.org/2007/papers/81.pdf)
Understanding how malware evades detection through code metamorphosis informs both offensive and defensive techniques.

14. [**BitBlaze: A New Approach to Computer Security via Binary Analysis**](https://bitblaze.cs.berkeley.edu/papers/bitblaze_iciss08.pdf)
Introduced automated binary analysis techniques combining symbolic execution, taint analysis, and emulation used in modern tools.

15. [**SoK: Shining Light on Shadow Stacks**](https://oaklandsok.github.io/papers/burrow2019.pdf)
Deep dive into shadow stack implementations, their effectiveness against control-flow hijacking, and limitations.

16. [**Off-Path Attacks on the TCP/IP Protocol Suite via Forged ICMP**](https://dl.acm.org/doi/10.1145/3689819)
Recent research showing new off-path attacks using ICMP errors—demonstrates modern protocol vulnerabilities still emerging.

17. [**CVAnalyzer: Automated Discovery of DoS Vulnerabilities in Connected Vehicle Protocols**](https://www.usenix.org/system/files/sec21-hu-shengtuo.pdf)
Shows systematic analysis of real-world IoT protocols and discovered 14 new vulnerabilities in connected vehicle systems.

18. [**Deep Learning Based Binary Code Analysis**](https://iris.uniroma1.it/retrieve/4218a45d-4904-4428-99b9-25bd6e25018c/Tesi_dottorato_Artuso.pdf)
Cutting-edge ML techniques for binary analysis including similarity detection and function identification—where the field is heading.

19. [**Hermes: Unlocking Security Analysis of Cellular Network Protocols by Synthesizing Finite State Machines**](https://www.usenix.org/system/files/usenixsecurity24-al-ishtiaq.pdf)
Uses AI to automatically generate formal models from natural language specs—found 3 new vulnerabilities in 4G/5G implementations.

20. [**The Illusion of Randomness: An Empirical Analysis of ASLR Effectiveness**](https://arxiv.org/abs/2408.15107)
Recent large-scale analysis showing ASLR entropy reduction on modern Linux versions and practical deanonymization techniques.

21. [**ARCTURUS: Full Coverage Binary Similarity Analysis with Reachability-guided Emulation**](https://dl.acm.org/doi/10.1145/3698900.3699214)
State-of-the-art binary diffing and similarity detection using reachability-guided emulation for vulnerability discovery.

22. [**WarpAttack: Bypassing CFI through Compiler-introduced Double-fetches**](https://nebelwelt.net/files/23Oakland3.pdf)
Shows how compiler optimizations can introduce new CFI bypasses through time-of-check-time-of-use vulnerabilities.

23. [**SMTP Smuggling: Breaking Email Authentication**](https://www.usenix.org/conference/usenixsecurity25/presentation/wang-chuhan)
Recent protocol-level attack bypassing SPF/DKIM/DMARC through SMTP protocol inconsistencies.

24. [**ClearAgent: Agentic Binary Analysis for Effective Vulnerability Detection**](https://scholar.google.com.pr/citations?user=D78jmV4AAAAJ&hl=iw)
Uses LLM agents for autonomous vulnerability discovery in binary code through agentic workflows.

25. [**Memory Safety Challenge Considered Solved? An In-Depth Study with All Rust CVEs**](https://zbchern.github.io/papers/tosem21.pdf)
Evaluates whether memory-safe languages actually solve the problem by analyzing all Rust CVEs for memory safety issues.

26. [**FirmLine: A Generic Pipeline for Large-Scale Analysis of IoT Firmware**](https://www.ndss-symposium.org/wp-content/uploads/bar2024-8-paper.pdf)
Modern scalable approach to analyzing IoT device firmware at scale with automated extraction and analysis.

27. [**SoK: (State of) The Art of War: Offensive Techniques in Binary Analysis**](https://sites.cs.ucsb.edu/~vigna/publications/2016_SP_angrSoK.pdf)
Ties together offensive binary analysis techniques and points to future research directions—great for inspiring your own projects.

