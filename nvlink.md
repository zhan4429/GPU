NVLink doesn't turn multiple GPUs into a single GPU; they're still multiple GPUs and you are free to schedule them independently. Even better, newer GPUs that support MIG let you treat one GPU as multiple GPUs for multiple different users.

As with most things in HPC, there is no magic that comes with NVLink. Your apps have to use the right libraries to make use of NVLink at all, and using those libraries doesn't prevent the same application from running on non-NVLink systems. The performance (bandwidth) will just be lower if you need to move data between GPUs.

Whether to buy NVLink or not really comes down to (1) how much speedup do you get from an NVLink-capable system vs. the equivalent PCIe-only system, and (2) how much extra does NVLink cost you? It's probably a lot (you must buy a 4-way or 8-way GPU board to get NVLink, and if one GPU fails, the whole board fails).

If you're doing any ML at all then NVLinks- and their beefier variant, NVSwitches- are amazing.

Although you can't use them to make two GPUs act as one, you can use them to enable larger batch sizes on your ML jobs (which is critical for NLP stuff) in a way that is significantly faster.

So it really depends- is this cluster going to be doing any machine learning working? If so then you should really consider the NVLink option.

One thing to note though is that NVLinks really only help if you have multiple GPUs on the same node (so they can work together) or if you have a really really beefy network (infiniband).


It really depends on what you're doing with these GPUs.
If you are training large models (computer vision models, some of the larger language models, etc) against large data and could benefit by using more GPU memory than a single card offers, then you'd probably benefit from some sort of high performance interconnect.

NVLink only works for 2 GPUs.

If you have an 8 GPU system, you can run 4 x 2GPU jobs efficiently, simultaneously.

If you want to run jobs wider than 2GPU, you should really look at getting a motherboard with nvswitch and moving to SXM4 GPUs.

That upgrade is probably around an extra ten thousand (shot in the dark pricing) if you were already spec'ing out an 8xGPU system.

Adding NVLink between the GPUs is pretty cheap. You're talking like an extra <$100 per pair (so like $400 max per server) on a system that is probably 100x that cost.
It does provide speed up for 2GPU jobs.

The problem with NVLinked pairs is scheduling. Someone requests 2 GPUs for a job, how do you guarantee that they get the NVLinked pair of 0&1, 2&3, etc? You're going to need to make weird custom scheduling slots. and it will make your system swiss cheese balancing that with the single GPU jobs that come in.

You're probably better off spending more to get NVswitch/Infiniband to interconnect the GPUs if that's really needed.

If you're just doing small scale ML models, add the $400 for the NVLink. If $400 is making or breaking this purchase, you shouldn't even be doing this.