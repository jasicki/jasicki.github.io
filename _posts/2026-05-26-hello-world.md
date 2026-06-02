---
title: "Downgrading iPhone X to iOS 14.3"
date: 2026-05-27 12:00:00 +0200
---

A guide for tether downgrading the iPhone X from latest iOS 16.7.16 to iOS 14.3 or other supported

## Disclaimer

I am not responsible for bricking your devices caused by following this guide. Please proceed with caution and at your own risk.

## Credits

* [Kaiden](https://kaiden.cc/posts/downgrading-the-iphone-5c-to-ios-6/) - For the original structure and inspiration for this tethered downgrade guide.
* [checkm8 exploit](https://github.com/axi0mX/ipwndfu) - For the bootrom vulnerability.
* [Tool Creator](https://github.com/example) - For the specific tethered boot utilities used in this guide.

## Requirements

* A Mac running macOS.
* A Lightning to USB-A cable (USB-C cables may cause issues entering DFU mode).
* Target IPSW file for iOS 14.3.
* Homebrew installed on your Mac.

## Preparation

1. Open Terminal and install dependencies:
```bash
   brew install libimobiledevice irecovery
   ```
2. Download the required downgrade script repository:
```bash
   git clone [https://github.com/example/downgrade-tool.git](https://github.com/example/downgrade-tool.git)
   cd downgrade-tool
   ```

## Creating the Custom IPSW

1. Move your downloaded iOS 14.3 IPSW into the `downgrade-tool` folder.
2. Run the extraction script. This will extract the `RootFS`, patch `iBSS` and `iBEC`, and repack it into a custom IPSW:
```bash
   ./create_custom_ipsw.sh iPhone10,6_14.3_18C66_Restore.ipsw
   ```

## Restoring the Device

1. Connect your iPhone X and put it into DFU mode:
   * Press and quickly release Volume Up.
   * Press and quickly release Volume Down.
   * Press and hold the Side button until the screen goes black.
   * Hold both the Side button and Volume Down for 5 seconds.
   * Release the Side button but keep holding Volume Down for another 10 seconds.
2. Run the checkm8 exploit to place the device into Pwned DFU state:
```bash
   ./ipwndfu -p
   ```
3. Flash the custom IPSW using the restore script:
```bash
   ./restore.sh custom_iPhone10,6_14.3_18C66_Restore.ipsw
   ```

## Tethered Booting

Since this is a tethered downgrade, your device will not boot on its own. You must use your computer to boot it every time it restarts.

1. Put the device back into DFU mode.
2. Run the boot script:
```bash
   ./boot.sh
   ```
3. Your iPhone X should now boot into iOS 14.3.

### Przykład kodu – Python

```python
import sys

def greet(name: str) -> str:
    """Zwraca powitanie dla podanej nazwy."""
    return f"Hello, {name}!"

if __name__ == "__main__":
    names = ["Alice", "Bob", sys.argv[1] if len(sys.argv) > 1 else "World"]
    for n in names:
        print(greet(n))
```

### Przykład kodu – Shell

```bash
# Sprawdź aktualną wersję gema Chirpy
bundle info --path jekyll-theme-chirpy

# Zbuduj stronę lokalnie
JEKYLL_ENV=production bundle exec jekyll build
```

### Przykład kodu – YAML (config snippet)

```yaml
# _config.yml
title: "My Blog"
theme: jekyll-theme-chirpy

assets:
  self_host:
    enabled: false
```

## Tabela

| Urządzenie     | Rok  | CPU              | RAM    |
|:---------------|:----:|:-----------------|-------:|
| iPhone 5c      | 2013 | Apple A6         | 1 GB   |
| Xbox 360       | 2005 | IBM Xenon        | 512 MB |
| Dell Optiplex  | 2016 | Intel i5-6500    | 8 GB   |

## Kod inline

W terminalu możesz użyć `bundle exec jekyll serve --livereload` żeby oglądać zmiany na żywo. Plik konfiguracyjny to `_config.yml`, a posty lądują w katalogu `_posts/`.

## Blockquote

> Dobry blog techniczny to nie kwestia platformy – to kwestia regularności i jakości treści.

To tyle na ten moment. Reszta postów pojawi się wkrótce.
