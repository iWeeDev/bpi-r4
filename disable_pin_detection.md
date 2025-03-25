Para deshabilitar la detección del PIN en un módem LTE en OpenWRT, como se menciona en la respuesta del foro, puedes hacerlo enviando comandos AT al módem. En este caso, el comando para deshabilitar la detección del PIN es:

```bash
AT+QSIMDET=0,0
```

Esto se puede hacer desde la terminal de OpenWRT o usando una interfaz que permita enviar comandos AT, como **ModemManager** o herramientas como **Minicom** o **Screen**.

### Pasos para hacerlo:
1. **Accede al módem a través de ModemManager o usando un terminal**.
   
   Si estás usando **ModemManager**:
   - Asegúrate de que tu módem esté correctamente detectado y que **ModemManager** lo esté manejando.
   - Puedes usar el comando `mmcli` para interactuar con el módem y enviar comandos AT.

2. **Enviar el comando AT para deshabilitar la detección del PIN**:
   - Puedes enviar el comando AT desde el terminal:
     ```bash
     echo -e "AT+QSIMDET=0,0" > /dev/ttyUSB0
     ```
     (Sustituye `/dev/ttyUSB0` por el nombre correcto del dispositivo de tu módem, dependiendo de cómo esté detectado en tu sistema).

3. **Reiniciar el módem (opcional)**:
   - El segundo comando mencionado en la respuesta es para reiniciar el módem:
     ```bash
     AT+CFUN=1,1
     ```

   Al enviar este comando, reiniciarás el módem. Puedes hacer esto de la misma manera a través del dispositivo serial correspondiente.

### Alternativa con **ModemManager**:
Si estás usando **ModemManager** y ya tienes tu módem detectado, también puedes enviar los comandos AT de forma sencilla:

1. Asegúrate de que **ModemManager** esté funcionando y detectando tu módem correctamente.
   
2. Usa **mmcli** para interactuar con el módem. Primero, lista los dispositivos conectados:
   ```bash
   mmcli -L
   ```

   Luego, para enviar comandos AT, utiliza:
   ```bash
   mmcli -m [ID del módem] --command="AT+QSIMDET=0,0"
   ```

   Si necesitas reiniciar el módem después:
   ```bash
   mmcli -m [ID del módem] --command="AT+CFUN=1,1"
   ```

De esta forma, deberías poder deshabilitar la detección del PIN y reiniciar el módem para continuar con la configuración.
