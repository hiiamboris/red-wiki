(NB: Just collecting commonly asked questions at the moment.)

# Technical

**Q. What do you mean by "full-stack language"? Isn't JS one?**



**Q. What about using LLVM as backend?**

This option was considered at the beginning, but quickly discarded as it would be a bad fit from several perspectives:

* Relying on LLVM, even if it has support for a broad range of features, would have limited the possible choices for Red's architecture and memory management options. 

* Red/System aims at providing full access to hardware features and be a good fit for building an operating system in the future.

* It requires to be able to statically link with LLVM libraries from the beginning and make the right functions and structures mappings. This would have taken more time than the direct path taken using a custom lightweight lower layer. (Note: Red has its own very small custom toolchain, it does not rely on Gcc or any other tool except for Rebol during the bootstrapping stage)

* LLVM adds several megabytes (5-10MB AFAICT) to the executable binary, this is something that we definitely want to avoid. We expect Red binary, with the JIT and Red/System compiler, to be in the 100-200KB range. You could think this is not relevant anymore, until you would need to upload your app to your favorite smartphone, pesting at how much time it takes and how much space it wastes, or trying to download such app through a low-band connection (not uncommun outside of the western world). 

* It is still possible to add LLVM as a target in the future, if this provides Red with a feature that we do not want or cannot afford to implement.