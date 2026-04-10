##
GlassWorm’s latest evolution is not just another malicious VS Code extension. According to Aikido’s April 8, 2026 research, the campaign now uses a Zig-compiled native addon to turn a single trojanized OpenVSX extension into a cross-editor infection mechanism that can silently push a second-stage implant into every compatible IDE it finds on the machine.

The malicious package, `specstudio/code-wakatime-activity-tracker`, impersonates the legitimate WakaTime developer productivity extension. On the surface, Aikido says it preserves the expected user experience — “same command registrations, same API key prompts, same status bar icons.” But that similarity is the cover, not the story.

The real action begins inside the extension’s `activate()` function. Before any legitimate extension logic runs, it loads a native Node addon from `./bin/win.node` on Windows or `./bin/mac.node` on macOS and immediately calls `install()`. That matters because native addons execute outside the JavaScript sandbox with direct operating-system access. In this case, Aikido reports both binaries were written in Zig, giving the attackers a stealthier and more portable mechanism than a plain JavaScript-only loader.

That native layer changes the scope of the intrusion. Instead of infecting just the IDE where the malicious extension was installed, the addon enumerates other VS Code-compatible editors present on the system and force-installs a malicious `.vsix` package into them. The affected editor family listed by Aikido includes Visual Studio Code, VS Code Insiders, Cursor, Windsurf, VSCodium, and Positron. In other words, a single bad extension install in one editor can become a multi-editor compromise across a developer workstation.

The second-stage payload is fetched from an attacker-controlled GitHub Releases URL and disguised as `autoimport-2.7.9.vsix`, impersonating the legitimate `steoates.autoimport` extension. Once downloaded, the installer uses each editor’s own CLI to push the package into every compatible IDE it discovers, then deletes the `.vsix` file afterward to reduce obvious evidence on disk.

This is the key escalation. Aikido says the new native installer “infects all other IDEs it can find on your system.” That is a materially different risk profile from a one-off malicious plugin. It turns extension trust into a lateral-movement opportunity inside the local developer environment.

The report also places this technique in the broader GlassWorm timeline. Aikido says it has tracked the actor since March 2025, when malicious npm packages used invisible Unicode characters to conceal payloads. The campaign later expanded into GitHub repositories, npm packages, OpenVSX extensions, and a fake Chrome extension used to deliver a persistent RAT. This new Zig-based stage shows that the actor is still adapting, and is now willing to use compiled native code to improve reliability and stealth.

For defenders, the takeaway is straightforward: if you find `specstudio/code-wakatime-activity-tracker` installed, or see the second-stage extension identifier `floktokbok.autoimport`, you should assume compromise, rotate exposed secrets, and inspect every compatible editor on the machine rather than only the one where the initial extension appeared. Aikido’s guidance is blunt: treat the host as compromised.

## IOC extraction from your attached files

### IP addresses (12 unique)

- `45.143.201.4`
- `66.171.248.178`
- `74.125.201.132`
- `93.184.221.240`
- `110.110.110.0`
- `110.110.110.3`
- `149.28.178.150`
- `193.106.191.146`
- `195.24.66.145`
- `195.201.225.248`
- `195.245.112.115`
- `199.59.243.228`

### SHA-256 hashes (53 unique)

- `0076ac146330511a82f5ce00a8667b06e6548deb514e8658a67afbc0b181c171`
- `014a6219fa87a0cb258437bdd815a74054aa8840c823d0358bec41d0e3b9348a`
- `01df115c5cbf2366510ab84edd45c243ec57b61faf4d358c9e108b8c18920a33`
- `02121d9c57a5ccc1a47c6847dd4ce8620b9def51b4bf7f29903055393391706d`
- `029b84a488259de4e886faff7981daa58ec5f676d141f42eacd3e9db72c6e49a`
- `03efc6c5d6d3854f20e2a3d7bdd55367238c2b3023ed33aad5f6f645940e4d0a`
- `04811adadff1d4e7acbac0d9f96a76915c5f39cf5af92f33ad794c831b3aec38`
- `04c8f2de54b18760e5ec42fd681501f871bbc11b22a69d924c4587d0c0196e05`
- `04df6e500e6b9bf72b485a856e94b1207e07b059b20d59fc5245ace810411db2`
- `0622506558032be8b4ae048850b475b335c22220cadc419f06403c54b65837f3`
- `063effe8ceab2a05b8243b521f7e65235ac7ae15fedec346ee5d0dea43cb435d`
- `06415479bb8d84aaa7f6b3ab1df9208e9d81c09c24bc020fa7cde31675acf021`
- `06866f635337a9591f8566dcf86819cfc0a1ba40f15544d048c969bbe0d2c5a8`
- `06f3c4f3fbf0e5453248580457a2d41bd7f387c2c16a917ac684e7de9972dee1`
- `07f7947fe04d80b97e4d6d248622cf8797c8b3cd9b581f49d29ad3aea6b81991`
- `0810f1e1b014228476c9a7f91d4202686d7509234ffa18cd43bf000336825eb3`
- `0836883e1edd3c96c6fc6792419e86e621fa4002d543627074a99fba029134f0`
- `083f72cc2ec1ac8fc746e05046d97ad32518d64206a4f9db3dc04ea5d5c72303`
- `086e3ada93c6cd654b89445eba35458a8e6028a96626d3dc8fda5a848731607f`
- `0940b6a7f5aed204277aa8c50694ea3e3d5a9e3c3be9e3ccf2a3edd0cf5f5777`
- `0947079aa24751b61c5d169696076f07678f2846d1c49eae06382337a9b9c7e8`
- `094d91f5d43f0b6f1ba70ab0efb252a6bd23399a4bb1a01bcdf013ee906a2bbd`
- `09ad72ac1eedef1ee80aa857e300161bc701a2d06105403fb7f3992cbf37c8b9`
- `09fb7bdeb400967a5a14709ffc3f757453f145e187619104d159d5ae55fc0e04`
- `0a08adb41640d5dc0789effce7987e5d5b7929c35b193c1a6acb62e06826fe8b`
- `0ccf879f8042839eab0b9bdef686ca6c71dc7d2e88875b2a626cea9cd01b7488`
- `0d58ccba4ce2fab4fa62a0066318bb62cb4da16767b502e4f5dbe175c4ce194e`
- `0e2ebfd289929981b9a4979af62e37fdcc60b993509553d5d960181bd50ce324`
- `0fe3f3060f76851e40212ac78a2449d25b3357ef729ded79e9a2aa7b795c7e49`
- `13d8743a593d157ac3c4eec2d379d06120127b3400c98be02ca596a887581334`
- `17373e4ebd4660cc83d12d8a73c46701ad560740a4829064a8544feab9cf9e2a`
- `17734ff29eec9c77dbda9aea37f8e85ec276d1599dc0162c569e637e17e86e42`
- `1ca0b2d66ef35d4bc3496012bcd248a7d8456622993656b45086adc9b92968e4`
- `1cb77c347a07670b56762a8e160c45a285c21af238a86cdf6113caed476666c3`
- `1ed02734c776296ca21b524a9a13bdae96ff77cac6be789135c56ad35bbdb021`
- `20ea9ea5c38024f985667863982eae930c0f92d2990a97e843c0b83a3ee94716`
- `2470c06586bbc7794f77bc8a95d608d5552e5a532a04faa33d77d2e3657d058a`
- `29b431808a394c9e0abc5aeafab108b03be7b9ecb728fd4894a45cbac6ca2f2c`
- `2a4b9dfd49b5e37d178be022fcba5bff1cbfeb3647c8f4048d201c8e370a7ef2`
- `2c97487d8557048f23e154128a606bc064723eb21e7cd24a04d760812920d954`
- `50a68eded53d94c230fc57232c3e26028ae726929c9c8ae7905cde469e74296f`
- `6dcd67d2704d9ebf659ea7a56db0b272747a2aab80d3cf57ef11a344c3059018`
- `83173d5fcc462820f09d5cbb8bddeea15b17c2c4e4fec50df6d8a9c92378582c`
- `8ff78d459a55378124a316b73f7ded9435582d805b8b4f984ae67983957c03eb`
- `985837f6d29f2fca78db956b2a8db882e13a7190c9002e64b72928af1978b7e9`
- `a031bc14dd0d6c64fa4c3877f9d3ad1c52562705b93e2c045d0cd9438303df26`
- `ac5f976b3ed31509a11232db9bbae2ea2126762a0659061cf9522e7b66d230b8`
- `b195a25a71de4eb55495de783c897dd897594cf5670f1aabaa6962255cb9343e`
- `e0365007b8347a8fa2bd7166463c1bd924cdbf0fa882a835af843b86d4462927`
- `e87a572245ed8628ec7fbcf0da2b91ad644f6b35e8788c8a529bcfa886629345`
- `e9641bf12b4e28c659197fa6e56d438dab3b50c2f25e3144e5c6a1d396f67683`
- `f3b22beb75e9c000869fdb683cef416ecdef4ac0cc78b9e04d886b5623905020`
- `f59216286e80ffd684b909cce8cb2fb2b6d0819eb6417539726af8cd1a738b41`

## YARA notes

I included a full YARA file separately because the hash list is large. It contains:
- one bulk hash rule covering all 53 SHA-256 values from your attached file
- one string rule for the Zig/native-addon installer artifacts
- one path-based rule for the multi-IDE spreader behavior
- one rule for embedded IP literals from your IOC list
- two exact-hash rules for Aikido’s published `win.node` and `mac.node` samples

### Important limitation

YARA is strongest for files and embedded strings. Plain IP addresses are usually better handled in:
- firewall blocklists
- EDR custom IOC feeds
- SIEM/Sigma detections
- Suricata/Snort network rules

The YARA IP rule I included only helps when those IPs appear as literal strings inside a sample.

## Related links and source pages

- [Aikido: GlassWorm goes native: New Zig dropper infects every IDE on your machine](https://www.aikido.dev/blog/glassworm-zig-dropper-infects-every-ide-on-your-machine)
- [Aikido: Glassworm Is Back: A New Wave of Invisible Unicode Attacks Hits Hundreds of Repositories](https://www.aikido.dev/blog/glassworm-returns-unicode-attack-github-npm-vscode)
- [Aikido: Invisible Unicode Malware Strikes OpenVSX, Again](https://www.aikido.dev/blog/invisible-unicode-malware-strikes-openvsx-again)
- [Socket: 72 Malicious Open VSX Extensions Linked to GlassWorm Campaign Now Using Transitive Dependencies](https://socket.dev/blog/open-vsx-transitive-glassworm-campaign)
- [Socket: GlassWorm Sleeper Extensions Activate on Open VSX, Shift to GitHub-Hosted VSIX Malware](https://socket.dev/blog/glassworm-sleeper-extensions-activated-on-open-vsx)
- [Socket: GlassWorm Loader Hits Open VSX via Developer Account Compromise](https://socket.dev/blog/glassworm-loader-hits-open-vsx-via-suspected-developer-account-compromise)
- [Legitimate WakaTime VS Code Marketplace page](https://marketplace.visualstudio.com/itemdetails?itemName=WakaTime.vscode-wakatime)
- [Legitimate Auto Import marketplace page](https://marketplace.visualstudio.com/items?itemName=steoates.autoimport)
- [Your original pasted draft](/mnt/data/Pasted%20text.txt)
- [Your attached IOC file](/mnt/data/IOCs-GlassWorm.csv)
- [Your attached hash file](/mnt/data/Glassworm-Files.csv)
