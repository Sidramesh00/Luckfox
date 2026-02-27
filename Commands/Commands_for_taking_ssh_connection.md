## Commands for taking ssh connection
### I removed and connected back the cable**
Ah, you just discovered the exact reason I suggested Path 1! 😂

When you unplugged and replugged the USB cable, the board rebooted. Because we haven't hardcoded a static MAC address into the board's OS yet, it generated a **brand new random MAC address** when it powered back up.

Because the MAC address changed, your Ubuntu PC saw it as a completely new device  and gave it a brand new `enx...` interface name. This means the IP address we assigned to the old name was wiped out, and the routing is broken again.

Here is how to get back in quickly so we can permanently fix it.

### Step 1: Find the New Interface Name

Run this on your PC's terminal to see what the new `enx...` name is:

```
ip a

```

### Step 2: Assign the IP & Wake It Up

Copy that new interface name from the bottom of the list and run these two commands (make sure to replace `enx_new_name` with your actual interface):

```
sudo ip addr add 172.32.0.100/24 dev enx_new_name
sudo ip link set dev enx_new_name up

```

### Step 3: SSH Back In

Now you can knock on the door again:

```
ssh pico@172.32.0.70

```

Once you are back at the `pico@luckfox:~$` prompt, let me know! We will immediately dive into the board's root file system to hardcode that MAC address so you can unplug this board as many times as you want without ever having to fight your PC's routing table again.
