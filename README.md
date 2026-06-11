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

Each step displays four things: a horizontal RAM bar where each program occupies a proportional colored segment, a stats row tracking total free memory, the largest contiguous free block, and the number of holes, a caption explaining what the OS is doing and why it matters, and a program queue showing the status of each program (Waiting, Next up, Running, Closed, or Failed).

Steps 0 through 4 load five programs into a clean 8 GB RAM bar one by one: Chrome with YouTube at 2 GB, Discord at 1 GB, Valorant at 3 GB, Spotify at 1 GB, and a Video Editor at 4 GB. Step 5 closes Chrome, leaving a 2 GB hole at the front. Step 6 closes Valorant, leaving a 3 GB hole in the middle. Discord (1 GB) remains open between the two large holes, physically blocking them from being merged. Step 7 shows the Video Editor failing to load: total free memory is 6 GB, but the largest single hole is only 3 GB, which is less than the 4 GB needed.

Steps 8 and 9 introduce virtual memory as the solution. Instead of a single bar, two bars are shown stacked. The top bar shows the physical RAM layout, still fragmented with Discord between the holes. The bottom bar shows Video Editor's virtual view, where the OS has used a page table to map Hole A (2 GB) and Hole B (3 GB) into one contiguous 5 GB address space. The program loads successfully, with no knowledge of the physical layout beneath it.

The component is built entirely in React with CSS transitions on the flex-grow values of memory bar segments for smooth resizing between steps.

**Mobile-responsive layout:**

* The RAM bar and stats row stack cleanly at narrow widths via CSS flex-wrap  
* Program queue cards use `auto-fit` grid columns, collapsing to 2 columns on mobile  
* Navigation buttons are full-touch-target height (minimum 44px) on small screens  
* Caption text reflows naturally — no horizontal scrolling at any viewport
