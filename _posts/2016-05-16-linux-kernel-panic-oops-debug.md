---
layout: post
categories: kernel
---

## Oops Code

```
Oops: 0002 [#1] PREEMPT SMP
```

This is the error code value in hex. Each bit has a significance of its own:

- bit 0 == 0 means no page found, 1 means a protection fault
- bit 1 == 0 means read, 1 means write
- bit 2 == 0 means kernel, 1 means user-mode
- [#1] — this value is the number of times the Oops occurred. Multiple Oops can be triggered as a cascading effect of the first one.

> http://www.linuxforu.com/2011/01/understanding-a-kernel-oops

```
CPU: 1 PID: 162 Comm: surfaceflinger Tainted: G        W  O 3.10.20-262458-ge1b992c #1
```

This denotes on which CPU the error occurred.

The Tainted flag points to P here. Each flag has its own meaning. A few other flags, and their meanings, picked up from kernel/panic.c:

- P — Proprietary module has been loaded.
- F — Module has been forcibly loaded.
- S — SMP with a CPU not designed for SMP.
- R — User forced a module unload.
- M — System experienced a machine check exception.
- B — System has hit bad_page.
- U — Userspace-defined naughtiness.
- A — ACPI table overridden.
- W — Taint on warning.

> http://www.linuxforu.com/2011/01/understanding-a-kernel-oops

```
5 # options: set env. variable AFLAGS=options to pass options to "as";
6 # e.g., to decode an i386 oops on an x86_64 system, use:
7 # AFLAGS=--32 decodecode < 386.oops
```

## Decode Code

```
# scripts/decodecode
$ cd kernel_src_folder
$ scripts/decode code < oops.txt
or
$ echo "Code: 16 01 00 00 39 45 08 72 60 8b 3d 40 09 fb c1 89 5d d4 eb \
  27 90 8d 74 26 00 8b 4e 18 81 f9 fc c3 de c1 0f 84 e9 00 00 00 8d 71 \
  e8 <8b> 49 e8 39 c1 0f 83 db 00 00 00 3b 45 08 77 20 8d 04 17 89 cb" \
  | scripts/decodecode
```

Output as below:

```
All code
========
   0:   16                      (bad)
   1:   01 00                   add    %eax,(%rax)
   3:   00 39                   add    %bh,(%rcx)
   5:   45 08 72 60             or     %r14b,0x60(%r10)
   9:   8b 3d 40 09 fb c1       mov    -0x3e04f6c0(%rip),%edi        # 0xffffffffc1fb094f
   f:   89 5d d4                mov    %ebx,-0x2c(%rbp)
  12:   eb 27                   jmp    0x3b
  14:   90                      nop
  15:   8d 74 26 00             lea    0x0(%rsi,%riz,1),%esi
  19:   8b 4e 18                mov    0x18(%rsi),%ecx
  1c:   81 f9 fc c3 de c1       cmp    $0xc1dec3fc,%ecx
  22:   0f 84 e9 00 00 00       je     0x111
  28:   8d 71 e8                lea    -0x18(%rcx),%esi
  2b:*  8b 49 e8                mov    -0x18(%rcx),%ecx         <-- trapping instruction
  2e:   39 c1                   cmp    %eax,%ecx
  30:   0f 83 db 00 00 00       jae    0x111
  36:   3b 45 08                cmp    0x8(%rbp),%eax
  39:   77 20                   ja     0x5b
  3b:   8d 04 17                lea    (%rdi,%rdx,1),%eax
  3e:   89 cb                   mov    %ecx,%ebx

Code starting with the faulting instruction
===========================================
   0:   8b 49 e8                mov    -0x18(%rcx),%ecx
   3:   39 c1                   cmp    %eax,%ecx
   5:   0f 83 db 00 00 00       jae    0xe6
   b:   3b 45 08                cmp    0x8(%rbp),%eax
   e:   77 20                   ja     0x30
  10:   8d 04 17                lea    (%rdi,%rdx,1),%eax
  13:   89 cb                   mov    %ecx,%ebx
```

## Where to Get Code

"Code: …" comes from kernel log Oops.

```
[166357.529863] BUG: unable to handle kernel paging request at ffffffe8
[166357.529901] IP: [<c131ef5a>] alloc_vmap_area.isra.20+0x12a/0x2c0
[166357.529924] *pdpt = 0000000001f1c001 *pde = 0000000001f20067 *pte = 0000000000000000 
[166357.529950] Oops: 0000 [#1] PREEMPT SMP 
[166357.529973] Modules linked in: atomisp_css2300 lm3554 mt9m114 ov8830 compat(O) rmi4 st_drv videobuf_vmalloc videobuf_core matrix(O) hdmi_audio pvrsgx wl12xx(O) mac80211(O) cfg80211(O) wl12xx_sdio(O) pnwdisp
[166357.530079] CPU: 1 PID: 6779 Comm: iptables Tainted: G        W  O 3.10.20-262458-ge1b992c #1
[166357.530090] Hardware name: Intel Corporation CloverTrail/FFRD, BIOS 406 2013.10.16:10.18.10
[166357.530102] task: cd195110 ti: cd5b8000 task.ti: cd5b8000
[166357.530117] EIP: 0060:[<c131ef5a>] EFLAGS: 00010213 CPU: 1
[166357.530134] EIP is at alloc_vmap_area.isra.20+0x12a/0x2c0
[166357.530145] EAX: e8db4000 EBX: 00000000 ECX: 00000000 EDX: e8db2000
[166357.530154] ESI: ffffffe8 EDI: 00001000 EBP: cd5b9dcc ESP: cd5b9d98
[166357.530165]  DS: 007b ES: 007b FS: 00d8 GS: 0033 SS: 0068
[166357.530175] CR0: 80050033 CR2: ffffffe8 CR3: 0d5b6000 CR4: 000007f0
[166357.530185] DR0: 00000000 DR1: 00000000 DR2: 00000000 DR3: 00000000
[166357.530195] DR6: ffff0ff0 DR7: 00000400
[166357.530204] Stack:
[166357.530212]  cd5b9dcc c132b08a 00002000 c75ca600 00000000 00000000 dec00000 ffffffff
[166357.530261]  dec00000 00000001 c75ca180 00000022 00000001 cd5b9dec c131f177 ffbfe000
[166357.530304]  000080d2 00001000 ffbfe000 80000000 ffffffff cd5b9e20 c131fe47 dec00000
[166357.530354] Call Trace:
[166357.530371]  [<c132b08a>] ? kmem_cache_alloc_trace+0xaa/0x170
[166357.530388]  [<c131f177>] __get_vm_area_node.isra.21+0x87/0x160
[166357.530402]  [<c131fe47>] __vmalloc_node_range+0x57/0x200
[166357.530417]  [<c18c3da6>] ? do_ipt_get_ctl+0x1a6/0x320
[166357.530431]  [<c1320052>] __vmalloc_node+0x62/0x70
[166357.530445]  [<c18c3da6>] ? do_ipt_get_ctl+0x1a6/0x320
[166357.530459]  [<c1320278>] vzalloc+0x38/0x40
[166357.530473]  [<c18c3da6>] ? do_ipt_get_ctl+0x1a6/0x320
[166357.530488]  [<c18c3da6>] do_ipt_get_ctl+0x1a6/0x320
[166357.530503]  [<c1460fc7>] ? avc_has_perm_flags+0xc7/0x170
[166357.530521]  [<c1862d10>] nf_getsockopt+0x40/0x60
[166357.530536]  [<c1883804>] ip_getsockopt+0x84/0xc0
[166357.530551]  [<c18a2982>] raw_getsockopt+0x32/0xb0
[166357.530567]  [<c1830e77>] sock_common_getsockopt+0x27/0x40
[166357.530582]  [<c183035e>] SyS_getsockopt+0x6e/0xe0
[166357.530598]  [<c1830be9>] SyS_socketcall+0x2b9/0x300
[166357.530615]  [<c14b88b8>] ? trace_hardirqs_on_thunk+0xc/0x10
[166357.530631]  [<c195e698>] syscall_call+0x7/0xb
[166357.530642] Code: 16 01 00 00 39 45 08 72 60 8b 3d 40 09 fb c1 89 5d d4 eb 27 90 8d 74 26 00 8b 4e 18 81 f9 fc c3 de c1 0f 84 e9 00 00 00 8d 71 e8 <8b> 49 e8 39 c1 0f 83 db 00 00 00 3b 45 08 77 20 8d 04 17 89 cb
[166357.530945] EIP: [<c131ef5a>] alloc_vmap_area.isra.20+0x12a/0x2c0 SS:ESP 0068:cd5b9d98
[166357.530971] CR2: 00000000ffffffe8
```

## Oops Tracing

`Tainted kernels` from kernel/Documentation/oops-tracing.txt

```
222 Tainted kernels:
223
224 Some oops reports contain the string 'Tainted: ' after the program
225 counter. This indicates that the kernel has been tainted by some
226 mechanism.  The string is followed by a series of position-sensitive
227 characters, each representing a particular tainted value.
228
229   1: 'G' if all modules loaded have a GPL or compatible license, 'P' if
230      any proprietary module has been loaded.  Modules without a
231      MODULE_LICENSE or with a MODULE_LICENSE that is not recognised by
232      insmod as GPL compatible are assumed to be proprietary.
233
234   2: 'F' if any module was force loaded by "insmod -f", ' ' if all
235      modules were loaded normally.
236
237   3: 'S' if the oops occurred on an SMP kernel running on hardware that
238      hasn't been certified as safe to run multiprocessor.
239      Currently this occurs only on various Athlons that are not
240      SMP capable.
241
242   4: 'R' if a module was force unloaded by "rmmod -f", ' ' if all
243      modules were unloaded normally.
244
245   5: 'M' if any processor has reported a Machine Check Exception,
246      ' ' if no Machine Check Exceptions have occurred.
247
248   6: 'B' if a page-release function has found a bad page reference or
249      some unexpected page flags.
250
251   7: 'U' if a user or user application specifically requested that the
252      Tainted flag be set, ' ' otherwise.
253
254   8: 'D' if the kernel has died recently, i.e. there was an OOPS or BUG.
255
256   9: 'A' if the ACPI table has been overridden.
257
258  10: 'W' if a warning has previously been issued by the kernel.
259      (Though some warnings may set more specific taint flags.)
260
261  11: 'C' if a staging driver has been loaded.
262
263  12: 'I' if the kernel is working around a severe bug in the platform
264      firmware (BIOS or similar).
265
266  13: 'O' if an externally-built ("out-of-tree") module has been loaded.
267
268 The primary reason for the 'Tainted: ' string is to tell kernel
269 debuggers if this is a clean kernel or if anything unusual has
270 occurred.  Tainting is permanent: even if an offending module is
271 unloaded, the tainted value remains to indicate that the kernel is not
272 trustworthy.
```

## Steps

```
1. Download vmlinux.bz2
2. bunzip vmlinux.bz2
3. objdump -C -S vmlinux > vmlinux.s
4. grep -n  ffffffff8206b829 vmlinux.s (RIP address)
5. sed -n 'mmmm,nnnnnp' vmlinux.s | vim -
```

