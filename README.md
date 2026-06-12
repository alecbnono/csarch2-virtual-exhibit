### **CSARCH2 VIRTUAL EXHIBIT**

### **Proposal Write Up Link:** https://docs.google.com/document/d/1ZMVh7Pd56G49Xbd0d0VN0TmAYVSYSMWPeGFJR1NeJHk/edit?tab=t.nmczb89685uj

### **Style Guide Snapshot Link:** https://www.figma.com/design/9UvQtgoi524cgPsaxZOKkg/CSARCH2---Style-guide-snapshot?node-id=1-1043&t=Wy7L57AGLdzWEqIN-1

**Submitted By:**

* Fabregas Matthew Drew  
* Lee Hannah  
* Lim Justin Lance Te  
* Nono Alec Marx Gabriel Belen  
* Yasumuro Mariel Mendoza

### **Background of the Proposed Virtual Exhibit**

In early multitasking systems, the OS carved physical RAM into chunks and handed them to processes as they started. When a process closed, its chunk was freed, but that freed region sat wherever it happened to be in memory, not necessarily adjacent to any other free region. Over time, as programs were started and stopped at different moments, free memory became scattered across small, non-contiguous holes. This is external fragmentation. This means a system can have enough total free memory to satisfy an allocation request, yet be completely unable to fulfill it because no single contiguous block is large enough. A program needing 2.5 GB cannot be split across a 1 GB hole and a 2 GB hole. It must land in one contiguous region.

The classical workaround is compaction, moving all loaded programs to one end of RAM to consolidate the holes into one large block. It works, but it is expensive: the OS must copy potentially gigabytes of data and can cause noticeable freezes in the process. **Virtual memory is the deeper solution.** By introducing a layer of indirection between virtual and physical addresses, the OS frees programs from needing contiguous physical memory entirely. Physical RAM is divided into small fixed-size pages that can be scattered anywhere, while the program sees a clean, continuous address space. The fragmentation problem disappears from the program's perspective.

**This exhibit uses fragmentation as the entry point to that story, letting visitors experience the problem firsthand before arriving at virtual memory as the answer.**

### **Tech Stack Plan**

The project will follow the provided Astro-based museum template to ensure compatibility with the central virtual museum website.

The proposed core stack is as follows:

* Node.js 26 for the required runtime environment  
* Astro 6 for the website framework and page structure  
* MDX for combining written exhibit content with interactive components  
* React JSX for building interactive visualizers and simulations  
* CSS Modules for scoped and organized component styling

### **Interactive element — Memory fragmentation step-through**

A two-phase interactive component is used.

In the first phase, the user builds a queue. They are presented with a set of programs, each with a fixed memory size: Chrome with YouTube at 2 GB, Discord at 1 GB, Valorant at 3 GB, Spotify at 1 GB, and a Video Editor at 4 GB. The user drags or clicks to arrange an event sequence, choosing which programs load first and which get closed partway through. A sixth slot lets the user pick one program to attempt loading at the end, which will either succeed or fail depending on the state of memory at that point.

In the second phase, the component generates a step-by-step presentation based on the queue the user built. Each step shows a horizontal RAM bar where each program occupies a proportional colored segment, a stats row tracking total free memory, the largest contiguous free block, and the number of holes, and a caption describing what the OS is doing at that step. The user navigates with Back and Next buttons.

Because the queue is user-defined, the fragmentation outcome varies. A user who closes programs in an order that leaves holes separated by a still-running program will hit the failure state. A user who closes programs cleanly from one end may not. This variability is intentional: it encourages visitors to experiment and discover the conditions that cause fragmentation rather than just observe a fixed example.

If the final load attempt fails due to fragmentation, the last two steps switch to a dual-bar view showing physical RAM on top and the program's virtual address space below, demonstrating how the OS uses a page table to map two separate physical holes into one contiguous virtual block for the program.

The component is built in React with queue state managed via useState hooks and the step sequence derived from the queue at the moment the user confirms it.


**Mobile-responsive layout:**

* The RAM bar and stats row stack cleanly at narrow widths via CSS flex-wrap  
* Program queue cards use `auto-fit` grid columns, collapsing to 2 columns on mobile  
* Navigation buttons are full-touch-target height (minimum 44px) on small screens  
* Caption text reflows naturally — no horizontal scrolling at any viewport
