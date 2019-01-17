# Simple FIFO and LRU Cache

[![][nuget-img]][nuget]

[nuget]:     https://www.nuget.org/packages/Caching.dll/
[nuget-img]: https://badge.fury.io/nu/Object.svg

The Caching library provides a simple implementation of a FIFO cache (first-in-first-out) and two LRU (least-recently-used) caches, one based on a dictionary and the second based on a BTree (CSharpTest.Net.Collections).  It is written in C# and is designed to be thread-safe.

Two projects are included in the solution:

- Caching: the cache classes
- CachingTest: a simple test client

Three caches are included; the use case for each is also listed:

- FIFOCache - First-in, first-out cache using a dictionary internally
- LRUCache - Least-recently used cache with a dictionary internally 
- LRUCacheBTree - uses a BTree (from CSharpTest.Net.Collections), suitable for larger cache sizes
 
## Usage

Add reference to the Caching DLL and include the Caching namespace:
```
using Caching;
```

Initialize the desired cache:
```
class Person
{
  public string FirstName;
  public string LastName;
}

FIFOCache<string, Person> cache = new FIFOCache<string, Person>(capacity, evictCount, debug);
LRUCache<string, Person> cache = new LRUCache<string, Person>(capacity, evictCount, debug)
LRUCacheBTree<string, Person> cache = new LRUCacheBTree<string, Person>(capacity, evictCount, debug);

// T1 is the type of the key
// T2 is the type of the value
// capacity (int) is the maximum number of entries
// evictCount (int) is the number to remove when the cache reaches capacity
// debug (boolean) enables console logging (use sparingly)
```

Add an item to the cache:
```
cache.AddReplace(key, data);
// key (T1) is a unique identifier
// data (T2) is whatever data you like
```

Get an item from the cache:
```
Person data = cache.Get(key);
// throws KeyNotFoundException if not present

if (!cache.TryGet(key, out data)) { // handle errors }
else { // use your data! }
```

Remove an item from the cache:
```
cache.Remove(key);
```

Other helpful methods:
```
T1 oldestKey = cache.Oldest();
T1 newestKey = cache.Newest();
T1 lastUsed = cache.LastUsed();  	// only on LRUCache
T1 firstUsed = cache.FirstUsed();   // only on LRUCache
int numEntries = cache.Count();
List<T1> keys = cache.GetKeys();
cache.Clear();
```
