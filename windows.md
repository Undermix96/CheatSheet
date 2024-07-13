## Shutdown Windows Remotely from Linux Host
```bash
net rpc shutdown -f -t <seconds-to-wait> -C 'Optional Popup Message' -U USERNAME%PASSWORD -I <host-ip>
```
