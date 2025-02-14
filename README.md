<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Monitoring Potensiometer</title>
    <script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>
</head>
<body>
    <h2>Data Potensiometer</h2>
    <p>Nilai Potensiometer: <span id="pot-value">--</span></p>

    <script>
        // Koneksi ke HiveMQ WebSocket MQTT
        const broker = "ws://broker.hivemq.com:8000/mqtt";
        const topic = "sensor/potensiometer";

        const client = mqtt.connect(broker);

        client.on("connect", () => {
            console.log("Connected to MQTT Broker!");
            client.subscribe(topic);
        });

        client.on("message", (topic, message) => {
            const data = JSON.parse(message.toString());
            document.getElementById("pot-value").innerText = data.pot;
        });
    </script>
</body>
</html>
