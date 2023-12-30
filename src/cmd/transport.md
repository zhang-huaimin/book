# 传输
## rsync
目录同步

```bash,editable
rsync -avh src/ user@ip:dst/

# 强制同步
rsync -avh --delete src/ user@ip:dst/
```