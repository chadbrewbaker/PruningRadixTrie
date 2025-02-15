PruningRadixTrie<br> 
[![MIT License](https://img.shields.io/github/license/wolfgarbe/pruningradixtrie.png)](https://github.com/wolfgarbe/PruningRadixTrie/blob/master/LICENSE)
========
**PruningRadixTrie - 1000x faster Radix trie** for prefix search & auto-complete

The PruningRadixTrie is a novel data structure, derived from a radix trie - but 3 orders of magnitude faster.

A [Radix Trie](https://en.wikipedia.org/wiki/Radix_tree) or Patricia Trie is a space-optimized trie (prefix tree).<br>
A **Pruning Radix trie** is a novel Radix trie algorithm, that allows pruning of the Radix trie and early termination of the lookup.

In many cases, we are not interested in a complete set of all children for a given prefix, but only in the top-k most relevant terms.
Especially for short prefixes, this results in a **massive reduction of lookup time** for the top-10 results.
On the other hand, a complete result set of millions of suggestions wouldn't be helpful at all for autocompletion.<br><br>
The lookup acceleration is achieved by storing in each node the maximum rank of all its children. By comparing this maximum child rank with the lowest rank of the results retrieved so far, we can heavily prune the trie and do early termination of the lookup for non-promising branches with low child ranks.

***

### Performance

![Benchmark](https://miro.medium.com/max/1400/1*HauBPEDRiwyQJ77OuJOj3g.png "Benchmark")
<br><br>
The **Pruning Radix Trie** is up to **1000x faster** than an ordinary Radix Trie.

While 36 ms for an autocomplete might seem fast enough for a single user, it becomes insufficient when we have to serve thousands of users in parallel. Then autocomplete lookups in large dictionaries are only feasible when powered by something much faster than an ordinary radix trie.

While a prefix of length=1 is not very useful for the Latin alphabet, it does make sense for CJK languages. Also, there are many more application fields for a fast prefix search algorithm beyond character-wise word completion: Instead of characters, the prefix can be composed of arbitrary items, e.g. 
whole words in a query completion, or towns in a long routing sequence.

### Dictionary

[Terms.txt](https://github.com/wolfgarbe/PruningRadixTrie/blob/master/PruningRadixTrie.Benchmark/terms.zip) contains 6 million unigrams and bigrams derived from English Wikipedia, with term frequency counts used for ranking. But you can use any frequency dictionary for any language and domain of your choice.

### Blog Posts
[The Pruning Radix Trie — a Radix trie on steroids](https://seekstorm.com/blog/pruning-radix-trie/)<br>
[1000x Faster Spelling Correction algorithm](https://seekstorm.com/blog/1000x-spelling-correction/)<br>
[SymSpell vs. BK-tree: 100x faster fuzzy string search & spell checking](https://seekstorm.com/blog/symspell-vs-bk-tree/)

### Application:
The PruningRadixTrie is perfect for auto-completion, query completion or any other prefix search in large dictionaries.
While 37 ms for an auto-complete might seem fast enough for a **single user**, it becomes a completely different story if we have to serve **thousands of users in parallel**. Then autocomplete lookups in large dictionaries become only feasible when powered by something much faster than an ordinary radix trie.

### Usage: 

**Create Object**
``` 
PruningRadixtrie pruningRadixTrie = new PruningRadixtrie();
``` 
**AddTerm:** insert term and term frequency count into Pruning Radix Trie. Frequency counts for same term are summed up.
```
pruningRadixTrie.AddTerm("microsoft", 1000);
```
**GetTopkTermsForPrefix:** retrieve the top-k most relevant terms for a given prefix from the Pruning Radix Trie.
``` 
string prefix="micro";
int topK=10;
var results = pruningRadixTrie.GetTopkTermsForPrefix(prefix, topK, out long termFrequencyCountPrefix);
foreach ((string term,long termFrequencyCount) in results) Console.WriteLine(term+" "+termFrequencyCount);
``` 
**ReadTermsFromFile:** Deserialise the Pruning Radix Trie from disk for persistence.
``` 
pruningRadixTrie.ReadTermsFromFile("terms.txt");
```
**WriteTermsToFile:** Serialise the Pruning Radix Trie to disk for persistence.
``` 
pruningRadixTrie.WriteTermsToFile("terms.txt");
```


### Ports
The following third party ports or reimplementations to other programming languages have not been tested by myself whether they are an exact port, error free, provide identical results or are as fast as the original algorithm. 

**Java**<br>
https://github.com/benldr/JPruningRadixTrie<br>

**Python**<br>
https://github.com/otto-de/PyPruningRadixTrie<br>

**Rust**<br>
https://github.com/peterall/pruning_radix_trie<br>

---

**PruningRadixTrie** is contributed by [**SeekStorm** - the high performance Search as a Service & search API](https://seekstorm.com)
