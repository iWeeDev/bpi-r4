To disable the SIM PIN detection on an LTE modem in OpenWRT you can do it by sending AT commands to the modem. In this case, the command to disable PIN detection is:

```bash
AT+QSIMDET=0,0
```

This can be done from the OpenWRT terminal or using an interface that allows you to send AT commands, such as **ModemManager** or tools like **Minicom** or **Screen**.

### Steps to do it:

1. **Access the modem via ModemManager or using a terminal**.

   If you are using **ModemManager**:
   - Ensure your modem is properly detected, and **ModemManager** is managing it.
   - You can use the `mmcli` command to interact with the modem and send AT commands.

2. **Send the AT command to disable PIN detection**:
   - You can send the AT command from the terminal:
     ```bash
     echo -e "AT+QSIMDET=0,0" > /dev/ttyUSB0
     ```
     (Replace `/dev/ttyUSB0` with the correct device name for your modem, depending on how it’s detected in your system).

3. **Restart the modem (optional)**:
   - The second command mentioned in the response is to restart the modem:
     ```bash
     AT+CFUN=1,1
     ```

   Sending this command will restart the modem. You can do this in the same way via the corresponding serial device.

### Alternative with **ModemManager**:
If you are using **ModemManager** and your modem is already detected, you can also send AT commands easily:

1. Ensure that **ModemManager** is working and detecting your modem properly.
   
2. Use **mmcli** to interact with the modem. First, list the connected devices:
   ```bash
   mmcli -L
   ```

   Then, to send the AT commands, use:
   ```bash
   mmcli -m [modem ID] --command="AT+QSIMDET=0,0"
   ```

   If you need to restart the modem afterward:
   ```bash
   mmcli -m [modem ID] --command="AT+CFUN=1,1"
   ```

This way, you should be able to disable the PIN detection and restart the modem to continue with the setup.
