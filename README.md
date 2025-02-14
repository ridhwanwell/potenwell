
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Potentiometer Monitor</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            text-align: center;
        }
        
        .gauge {
            width: 200px;
            height: 200px;
            margin: 20px auto;
            border-radius: 50%;
            border: 10px solid #ddd;
            position: relative;
            overflow: hidden;
        }
        
        .gauge-fill {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background-color: #4CAF50;
            transition: height 0.3s ease-in-out;
        }
        
        .value {
            font-size: 24px;
            font-weight: bold;
            margin-top: 20px;
        }
        
        .status {
            margin-top: 10px;
            color: #666;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Potentiometer Monitor</h1>
        <div class="gauge">
            <div class="gauge-fill" id="gaugeFill"></div>
        </div>
        <div class="value">
            Value: <span id="potValue">0</span>%
        </div>
        <div class="status" id="connectionStatus">
            Disconnected
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/mqtt/4.3.7/mqtt.min.js"></script>
    <script>
        const mqttTopic = 'sensor/potentiometer';
        const broker = 'wss://broker.hivemq.com:8884/mqtt';
        
        let client = mqtt.connect(broker);
        
        client.on('connect', function() {
            console.log('Connected to MQTT broker');
            document.getElementById('connectionStatus').textContent = 'Connected';
            document.getElementById('connectionStatus').style.color = '#4CAF50';
            
            client.subscribe(mqttTopic);
        });
        
        client.on('message', function(topic, message) {
            if (topic === mqttTopic) {
                const value = parseInt(message.toString());
                updateGauge(value);
            }
        });
        
        client.on('error', function(error) {
            console.log('Connection error:', error);
            document.getElementById('connectionStatus').textContent = 'Connection Error';
            document.getElementById('connectionStatus').style.color = '#f44336';
        });
        
        function updateGauge(value) {
            document.getElementById('potValue').textContent = value;
            document.getElementById('gaugeFill').style.height = value + '%';
        }
    </script>
</body>
</html>
