# Interrupts-softirqs-workqueues
Interrupts, softirqs/bottom halves, exceptions, workqueues

**Interrupts:**
1) When you type a button on the keyboard, the keyboard will send an interrupt/electrical signal to the CPU. CPU then notifies to the Operating System (OS). The OS will handle the
interrupt appropriately.

2) It is asychronous wrt the processor clock.

3) IRQ: Interrupt ReQuest/Interrupt ReQuest lines: These are the numbers given to the Interrupts.
Eg:
On a PC, 0 is timer interrupt and 1 is the keyboard interrupt.

**Exceptions:**
They are interrupts but they are synchronous wrt processor clock.
Eg: Divide by 0.

Eg for Software interrupts: systemcall. open. They will trap into the linux kernel and execute a special system call handler.

4) They execute in a special context called "interrupt context". It is also called "atomic context" because code executing in this context is unable to block.

You need to quickly exit the interrupt context, because you have may get more interrupts!, or you might have interrupted an interrupt since interrupt is asynchronous.

Interrupts have their own stack called 'Interrupt stack'. Each core has 1 stack (1 page, i.e. 4KB for 32 bit mackine and 8KB for 64 bit machine).


5) Top Halves/bottom Halves:
   Eg: 10G (gigabit) Ethernet card.
   When the packet comes, the interrupt is raised, the packet is copied from the networking hardware/network card to the memory. It should be processed and push it to the
   appropriate protocol stack. This is lot of work. therefore the complete thing cannot be done in one-shot in the top half hence it is divided into top half and bottom
   half. In the top half only the important thing will be done and the rest is deferred to the bottom half.

   Top half: copy the packet to the main memory, and reset the network card to receive more packets.
   Bottom half: To process the packet and send it to the appropriate stack to process the packet.

   The bottom half is called softirq.

   ```
   cat /proc/interrupts

   It shows the number of interrupts received on each core
   ```
