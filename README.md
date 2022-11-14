
# GTA V Reversing Notes

A record of my findings while reversing GTA V




## Sections

- Behaviours
- Signatures
- Structs


## Signatures
Native Namespace Start - Top of the registration chain for each namespace.
```
48 83 EC 28 48 8D 15 ? ? ? ? 48 B9 ? ? ? ? ? ? ? ? E9
```
Get Native Handler - Pointer to native registry and call to GetNativeHandler
```
48 8D 0D ? ? ? ? 48 8B 14 FA E8 ? ? ? ? 48 85 C0 75 0A
```
## Structs
scrNativeRegistry:
```cpp
class scrNativeRegistry
{
public:
    scrNativeRegistryEntry* entries[0xFF];
    std::uint32_t _unk;
    bool is_initialised;
}
```

scrNativeRegistryEntry
```cpp
class scrNativeRegistryEntry
{
public:
    std::uint32_t next_hashes[3];
    std::uint32_t _unk;
    void* handlers[7];
    std::uint32_t count_hashes[2];
    std::uint32_t _unk0;
    std::uint32_t native_hashes[7][3];
}
```

CPedFactory
```cpp
class CPedFactory
{
public:
    void** _vft;
    CPed* local_ped;
}
```
