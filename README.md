
# GTA V Reversing Notes

A record of my findings while reversing GTA V




## Sections

- Behaviours
- Signatures
- Structs
- Natives


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

## Native Notes
### PLAYER::PLAYER_ID
Works by accessing the local net player from `CNetGamePlayerMgr + 0xE8` then reading the player id from the net player at `0x21`
### PLAYER::GET_PLAYER_PED_SCRIPT_INDEX
Works by reading the local ped from `CPedFactory + 0x8` but if the player id passed is non-zero and the game is online then it retrieves the net player from the array found in `CNetGamePlayerMgr + 0x180` then accessing the CPlayerInfo at `CNetGamePlayer + 0xA0` from there it reads the ped ptr at `CPlayerInfo + 0x1E8`

