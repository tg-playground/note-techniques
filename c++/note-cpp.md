# C++ Notes

Iterator Map

```cpp
for (std::pair<const char,int>& x: mymap) 
{
    std::cout << x.first << " => " << x.second << '\n';
} 
```

C++11 Features: Range-Based for Loops, **Auto**: Automatic Type Deduction with auto

```cpp
for (auto it=mymap.begin(); it!=mymap.end(); ++it)
{
    
}
```

-

```cpp
for (auto x: mymap) {
  cout << x.first << endl;
}
```

C++17

```cpp
for (auto& [key, value]: mymap) {
        std::cout << key << " => " << value << '\n';
} 
```



