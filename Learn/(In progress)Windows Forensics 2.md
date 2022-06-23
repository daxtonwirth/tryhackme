# Windows Forensics 2
## 6/18/22

- Registry is great, but there are other places to find artifacts
- Hard drives are organized by file systems including 
  - FAT (File Allocation Table): table of indexes of where files are located
    - fat12,16,32 increased cluster and volume size
    - Used mainly for USB,SD (Does not include unnecessary security features)
  - exFAT: default for SD larger than 32 GB, reduced some overhead of FAT and max of 128 PB
