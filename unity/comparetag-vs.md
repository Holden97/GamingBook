# CompareTag vs ==

Compare tag is faster and don’t allocate memory. Unity say: “Another unexpected cause of heap allocations can be found in the functions GameObject.name or GameObject.tag. Both of these are accessors that return new strings, which means that calling these functions will generate garbage. Caching the value may be useful, but in this case there is a related Unity function that we can use instead. To check a GameObject’s tag against a value without generating garbage, we can use GameObject.CompareTag().”

[https://unity3d.com/learn/tutorials/topics/performance-optimization/optimizing-garbage-collection-unity-games?playlist=44069](https://unity3d.com/learn/tutorials/topics/performance-optimization/optimizing-garbage-collection-unity-games?playlist=44069)
