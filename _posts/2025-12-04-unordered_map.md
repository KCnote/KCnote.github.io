---
layout: post
title: "Hash Table - unordered_map"
date: 2025-12-04 12:00:00 +0900
author: kang
categories: [structure, hash]
tags: [structure, algorithm, std, hash, find, optimization]
pin: false
math: true
mermaid: true
---

# Hash
- When we want to map between key and value, we can make hash table. Especially we should optimize the search, insert, delete. (All of time complexity is O(1)) In this post, I show the unordered_map and make Hash Table by myself like unordered_map.

#### unordered_map

```cpp
	int arrI32List[] = { 5,-3,7,41,2 };
	int i32Size = (int)sizeof(arrI32List) / sizeof(int);

	std::unordered_map<int, int> mapList;
	
	for (int i = 0; i < i32Size; ++i)
	{
	  if (mapList.find(arrI32List[i]) == mapList.end())
		  mapList.insert({arrI32List[i], i});
	}

	if (mapList.find(41) != mapList.end())
		mapList.erase(41);

	std::unordered_map<int, int>::iterator iter1 = mapList.find(7);
	std::pair<const int, int> iter2 = *mapList.find(7);
	auto iter3 = mapList.find(7);

	int key = iter3->first;
	int value = iter3->second;
```

#### Insert, find, erase -> time complexity O(1)



### Implementation
#### Make HashTable by myself like unordered_map

```cpp
#include <iostream>
#include <vector>

class HashTable
{
public:
	HashTable();

public:
	bool Insert(int i32Key, int i32Value);
	bool Erase(int i32Key);
	bool Find(int i32Key, int& i32Value);
	bool IsFind(int i32Key);

private:
	int MakeHash(int i32Key);

public:
	std::vector<std::vector<std::pair<int, int>>> vctTable;
};

HashTable::HashTable()
{
	vctTable.resize(16);
}

bool HashTable::Insert(int i32Key, int i32Value)
{
	int i32Idx = MakeHash(i32Key);

	vctTable[i32Idx].emplace_back(i32Key, i32Value);

	return true;
}

bool HashTable::Erase(int i32Key)
{
	int i32Idx = MakeHash(i32Key);
	std::vector < std::pair<int, int>>& vctData = vctTable[i32Idx];

	for (int i = 0; i < (int)vctData.size(); ++i)
	{
		if (vctData[i].first == i32Key)
		{
			vctData[i] = vctData.back();
			vctData.pop_back();
			break;
		}
	}

	return true;
}

bool HashTable::Find(int i32Key, int& i32Value)
{
	bool bReturn = false;

	do
	{
		int i32Idx = MakeHash(i32Key);
		
		std::vector < std::pair<int, int>>& vctData = vctTable[i32Idx];

		for (int i = 0; i < (int)vctData.size(); ++i)
		{
			if (vctData[i].first == i32Key)
			{
				i32Value = vctData[i].second;
				bReturn = true;
				break;
			}
		}


	} 
	while (false);

	return bReturn;
}

bool HashTable::IsFind(int i32Key)
{
	int i32Dummy;

	return Find(i32Key, i32Dummy);
}

int HashTable::MakeHash(int i32Key)
{
	int i32Size = (int)vctTable.size();

	return (i32Key % i32Size + i32Size) % i32Size;
}

int main()
{
	int arrI32List[] = { 5,-3,7,41,2 };
	int i32Size = (int)sizeof(arrI32List) / sizeof(int);

	HashTable mapList;
	
	for (int i = 0; i < i32Size; ++i)
	{
		if (!mapList.IsFind(arrI32List[i]))
			mapList.Insert(arrI32List[i], i);
	}

	if (mapList.IsFind(41))
		mapList.Erase(41);

	int i32Value;
	bool bFind = mapList.Find(7, i32Value);

	return 0;
}

```