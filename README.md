# Barletta Hook ⭐
![Logo](https://i.postimg.cc/pd86x25W/barlihook4.png)
###### Thanks to @1glc for the banner
#### Barletta Hook is a project made to exploit an old Discord file, accesing direct audio acces and many more.




## Authors

- [@ghostof1337](https://www.github.com/ghostof1337projects)




## Installation

* Install the [C++ build tools](https://aka.ms/vs/17/release/vs_buildtools.exe) (Or run ``install_vs_buildtools.bat``)
* Open the ``barletta hook.sln``
* Select __Release__ and not __Debug__
* Click on __Build__
* Click __Build Solution__
* Go to ``\AppData\Local\Discord\app-\modules\discord_voice-1\discord_voice`` and replace the ``discord_voice.node`` file with the one in the project
* Replace also the openh DLL with ``openh264-2.2.0-win64.dll`` (for stereo support)
* Run the injector
* Open Discord

## Features
##### Can change the following:

- Made with IMGUI
- Normal gain (1-90)
- Rage gain (1-120)
- vUnits gain (1-99999997952)
- Spoofed gain (1-25)
- Bitrate (16k-510k)
- Audio content type
- Encoding method (VBR/CBR/CVBR)
- Opus types (SILK/CELT/HYBRID)
- Channel more (Stereo/Mono)
- Opus frame size changer (120-2880)
- EQ (Bass/Pierce/Wide)
- Reverb
- Panning (Left/Right/In Head)
- dB/LUFS checker (Cool spectrogram)
- Hotkey changer
- Streamproof 
- RGB mode
- Console output settings (dB checker)
- Keep window on top
- Change color for UI
- Shows injected process infos
- Gets your windows version, username and build date
- Shows who made the hook
- Shows which key is set
- Killswitch method with pastebin
- XOR,SkCrypt,rotCrypt encryption methods
- Hardclipper/SpoofdB detection (90% accuracy)

## Opus hooking function

```C++
HMODULE WINAPI HookedLoadLibraryExW(LPCWSTR moduleName, HANDLE file, DWORD flags) { // loads the dll
    if (!wcsstr(moduleName, L"discord_voice")) { // searches for the discord_voice.node file
        return g_OriginalLoadLibraryExW(moduleName, file, flags);
    }

    HMODULE voiceModule = g_OriginalLoadLibraryExW(moduleName, file, flags);
    if (!voiceModule) {
        return nullptr;
    }

    const uintptr_t voiceEngine = reinterpret_cast<uintptr_t>(voiceModule);
    
    auto HookFunction = [voiceEngine](uintptr_t offset, LPVOID detour, LPVOID* original = nullptr) {
        return MH_CreateHook(reinterpret_cast<LPVOID>(voiceEngine + offset), detour, original) == MH_OK;
    };
    
    XorS(hookStatus, "Hook ");
    
    const bool hookSuccess = HookFunction(0x863E90, custom_opus_encode) &&  // encode opus (gain)
                             HookFunction(0x867BA0, dbchecker, (void**)&opusdecode_orig);  // decode opus (db checker)
    
    if (!hookSuccess) {
        printf("hook failed\n");
    }
```
## Other hooking methods (for EQ, Reverb, ect)

```C++
    struct FunctionHook {
        uintptr_t offset;
        LPVOID detour;
        LPVOID* original;
        const char* name;
    };

    const FunctionHook hooks[] = {
        {0x46869C, reinterpret_cast<LPVOID>(ReturnZero), nullptr, "High Pass Filter"},
        {0x2EF820, reinterpret_cast<LPVOID>(ReturnZero), nullptr, "ProcessStream AudioFrame"},
        {0x2EDFC0, reinterpret_cast<LPVOID>(ReturnZero), nullptr, "ProcessStream StreamConfig"},
        {0x2F2648, reinterpret_cast<LPVOID>(ReturnZero), nullptr, "SendProcessedData"},
        {0x5D8750, reinterpret_cast<LPVOID>(ReturnZero), nullptr, "Clipping Predictor"},
        {0x2E923C, reinterpret_cast<LPVOID>(SplittingFilterHook), reinterpret_cast<LPVOID*>(&g_OriginalSplittingFilter), "Splitting Filter"},
        {0x2E8F00, reinterpret_cast<LPVOID>(MatchedFilterHook), reinterpret_cast<LPVOID*>(&g_OriginalMatchedFilter), "Matched Filter"}
    };
```

## Screenshots
![image](https://i.postimg.cc/4dBmCjCN/ui.png)
## How to get the offset




## 🔗 Links
[Imgui](https://github.com/ocornut/imgui)
[Skcrypt](https://github.com/SkyCryptWebsite/SkyCrypt)
[XOR](https://github.com/JustasMasiulis/xorstr)
[rotCrypt](https://github.com/WAQQASSX/RotCrypt)
[]()

## Licences


[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![GPLv3 License](https://img.shields.io/badge/License-GPL%20v3-yellow.svg)](https://opensource.org/licenses/)
[![AGPL License](https://img.shields.io/badge/license-AGPL-blue.svg)](http://www.gnu.org/licenses/agpl-3.0)


## Note

###### This is a Discord cheat exploit, IM NOT RESPONSIBLE FOR YOUR ACTIONS! USE IT AT YOUR OWN RISK
###### This source was originally made from ``afy source``.
