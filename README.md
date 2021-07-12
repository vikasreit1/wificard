# wificard

If you want some interactivity (bash):
    read -rp "SSID: " ssid
    read -rsp "Password: " pass
    echo -e "\n"
    qrencode -t utf8 "WIFI:T:WPA;S:$ssid;P:$pass;;"
    echo "SSID: $ssid"
Even better, generate a PDF with emojis and all!
    #!/usr/bin/env bash
    out="${1:-wifi-card.pdf}"
    read -rp "SSID: " ssid
    read -rsp "Password: " pass
    echo -e "\nGenerating PDF..."
    {
    cat << EOF
    <table>
    <tr>
    <td><img src="data:image/png;base64,$(qrencode -o - -t png "WIFI:T:WPA;S:$ssid;P:$pass;;" | base64)"></td>
    <td><span>SSID: $ssid</span><br><span>Password: $pass</span></td>
    </tr>
    </table>
    <p>
    <img width=16 height=16 src="https://raw.githubusercontent.com/iamcal/emoji-data/master/img-apple-64/1f4f8.png">
    <img width=16 height=16 src="https://raw.githubusercontent.com/iamcal/emoji-data/master/img-apple-64/1f4f1.png">
    Point your phone's camera at the QR Code to connect automatically.
    </p>
    EOF
    } | pandoc --pdf-engine=xelatex -f html -t pdf -o "$out"
    echo "$out"
