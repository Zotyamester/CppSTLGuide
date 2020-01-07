# Getting started with STL in C++
---
*This guide is intended to help you get started with Standard Template Library in C++*
The Standard Template Library is one of the reasons why C++ lets you write programs much faster than how you could do that in C.

*Woah... Did I just say C?*
Just to make it clear. Sorry about that, but I'll mention it quite a lot of times through the guide, because I like
comparing C++'s features to C's.

It has tons of great features such as strings, data structures, algorithms, iterators and much more.

I divided this guide into 5 sections:
- **Strings**
- **Containers**
- **Algorithms**
- **Iterators**
- ***Complex example***
- ***Final thoughts***

## Strings
String is a must.
There are almost no programs that don't involve strings in it somehow.
Even if it's for a single print or scan it's **needed**.
And I feel like I'm repeating myself, but it is one of things where C++ really shines.

Imagine how you can concat two strings by creating another one in C:
```
char *new_str = malloc(strlen(str1) + strlen(str2) + 1);
if (!new_str)
  goto ERROR; // it's more like the (T)ERROR of C
strcpy(new_str, str1);
strcat(new_str, str2);

ERROR:
```
The same in C++ using STL:
```
std::string new_str = str1 + str2; // sure, str1 must be an std::string, but you get the idea
```

It really gives you everything that you could probably think of (or what you'll ever need).
It has:
- dynamic resizing, you'll never have to use malloc again
- takes O(1) to get the length of it, because it keeps track of the current size
- supports reading/writing through the stdio

## Containers
In this section I further divided the topic into subcomponents.
The thing that is the same in all of them is that every single one is a template.
So you can store any kind of data structures in these.
Don't worry! It's really easy to understand.
### std::vector
If the name doesn't mean anything to you, let me put it this way: dynamic array.
Its size can be extended anytime unlike regular arrays'.
Here's an example showing the basics of std::vector:
```
std::vector<int> vec(10); // pre-defining how many items to have by default
for (int i = 0; i < 10; i++)
  vec[i] = i + 1;
vec.pop_back();
vec.reverse();
vec.push_back(0);
// 9, 8, 7, 6, 5, 4, 3, 2, 1, 0
```
### std::list
Lists are generally unpreferred in C++ since they are implemented as linked lists.
Though ff you have to delete lots of (big) items it is still better.
Here's an example:
```
std::list<std::string> words = {"nothing"};
words.pop_back();
words.push_back("C++");
words.push_back("test");
words.pop_back();
words.push_front("love");
words.push_front("I");
// I love C++
```

### std::map and std::set
If you know Python then map is the new dict and set remains set.
If not, then I'll explain a bit.
Map lets you store values by keys.
Another example:
```
std::map<std::string, int> entries = {
  {"C++", 123},
  {"Assembly", 9},
  {"C", 1000}
};
entries["python"] = 120;

auto entry = entries.find("C");
if (entry != entries.end())
  // found, access data by *entry
else
  // not found
```
Set stores values in a way, that it'll be easy to find an element when you need to.
It suppports insertion as the following:
```
std::set<int> s = {2, 3};
s.insert(1);
s.insert(1); // does nothing, 1 is already in the set
s.insert(10);
```
And searching for the elements works the same as with maps.
Both sets and maps are pretty fast when an element when you have to search in the container.

## Iterators
There are iterators for every mentioned containers and even for strings.
To iterate over the elements of a container you have to get the first and the last iterator of it.
```
start = iterable.begin();
end = iterable.end();
```
Let's see an example of iterating over a vector:
```
std::vector<int> v = {1, 2, 3, 4};
for (auto it = v.begin(); it != v.end(); it++)
  // do something, you can access the item as *it
```

## Algorithms
C++ also has template algorithms in the standard library.
It also has lots of functions so I'll just show a few of them.
Here's an example showing the usage of **min**, **max** and **sort**:
```
std::string a = "Assembly", "Basic";
std::min(a, b);
...
int a = 5, b = 3;
std::max(a, b);
...
std::vector<int> v = {1, 5, 3, 4, 2};
std::sort(v.begin(), v.end());
```
It's also worth mentioning that most of the standard algorithms are based on iterators (which we discussed earlier).

## Complex example
```
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <algorithm>
#include <numeric>

using namespace std; // for the sake of simplicity

vector<double> GetData()
{
  int count;
  
  cout << "How many entries would you like to enter, sir?" << endl;
  cin >> count;
  
  vector<double> data(count);
  
  for (int i = 0; i < count; i++) {
    cout << "Entry " << (i + 1) << ": ";
    cin >> data[i];
  }
  
  return data;
}

double GetAverage(const vector<double>& data)
{
  return accumulate(data.begin(), data.end(), 0.0) / data.size();
}

double GetMode(const vector<double>& data)
{
  map<double, int> freq;
  
  for (auto it = data.begin(); it != data.end(); it++)
    freq[*it]++;
  
  auto max_it = max_element(freq.begin(), freq.end(),
    [] (const pair<double, int>& p1, const pair<double, int>& p2) {
      return p1.second < p2.second;
    }
   );
  
  return max_it->first;
}

// Side effect: Sorts the vector
double GetMedian(vector<double>& data)
{
  sort(data.begin(), data.end());
  int size = data.size();
  if (size % 2 == 0)
    return (data[size / 2] + data[size / 2 - 1]) / 2.0;
  else
    return data[size / 2];
}

int main()
{
  vector<double> data = GetData();
  double average = GetAverage(data);
  double mode = GetMode(data);
  double median = GetMedian(data);
  cout << "Average: " << average << endl;
  cout << "Mode: " << mode << endl;
  cout << "Median: " << median << endl;
}
```

## Final thoughts
So far so good. But there's more. (*winky face*)
One of the most hated things in C is dealing with pointers.
If you never had such problems, lucky you. But even if you had, it's no longer a thing. (well part of)
C++ gives 'new kinds of pointers', these are not to be confused with the meaning which we used to say in C or Assembly,
because these are all wrapper classes for the 'standard pointers'.
It can help your workflow by for example not letting you to have multiple references to the same pointer.
If you're curious about these, just Google it. You'll find plenty of resources.
Here are the keywords to use: std::unique_ptr, std::shared_pointer, std::auto_pointer, std::weak_pointer.

*Author: Zoltán Szatmáry*
