
<html lang="fr">
<head>
    <meta charset="UTF-8" />
   
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
        :root {
            --primary-color: #4a6fa5;
            --secondary-color: #ffc0cb;
            --accent-color: #ff8c94;
            --light-color: #f8f9fa;
            --dark-color: #343a40;
            --border-radius: 8px;
            --box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 0;
            padding: 20px;
            background-color: #f5f7fa;
            color: var(--dark-color);
            line-height: 1.6;
        }

        h2 {
            color: var(--primary-color);
            margin-bottom: 25px;
            text-align: center;
            font-size: 2rem;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
        }

        canvas {
            border: 2px solid var(--primary-color);
            border-radius: var(--border-radius);
            cursor: crosshair;
            margin-top: 20px;
            box-shadow: var(--box-shadow);
            background-color: white;
            max-width: 100%;
        }

        .controls {
            display: flex;
            gap: 15px;
            align-items: center;
            margin: 20px 0;
            flex-wrap: wrap;
            justify-content: center;
            background-color: white;
            padding: 15px;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            width: 90%;
            max-width: 800px;
        }

        label {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-weight: 500;
            color: var(--primary-color);
            gap: 5px;
        }

        input[type="color"] {
            width: 40px;
            height: 40px;
            border: 2px solid var(--primary-color);
            border-radius: 50%;
            cursor: pointer;
            padding: 0;
        }

        input[type="range"] {
            width: 100px;
            cursor: pointer;
        }

        button {
            padding: 10px 20px;
            font-size: 14px;
            cursor: pointer;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: var(--border-radius);
            transition: all 0.3s ease;
            font-weight: 500;
            box-shadow: var(--box-shadow);
        }

        button:hover {
            background-color: var(--accent-color);
            transform: translateY(-2px);
        }

        button:active {
            transform: translateY(0);
        }

        #upload {
            display: none;
        }

        .upload-btn {
            padding: 12px 24px;
            background-color: var(--primary-color);
            color: white;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-weight: 500;
            margin-bottom: 15px;
            box-shadow: var(--box-shadow);
            transition: all 0.3s ease;
        }

        .upload-btn:hover {
            background-color: var(--accent-color);
        }

        #clear {
            background-color: #dc3545;
        }

        #clear:hover {
            background-color: #c82333;
        }

        #download {
            background-color: #28a745;
        }

        #download:hover {
            background-color: #218838;
        }

        @media (max-width: 768px) {
            .controls {
                flex-direction: column;
                align-items: stretch;
            }

            label {
                flex-direction: row;
                justify-content: space-between;
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <h2>colorer maintenant</h2>

    <label for="upload" class="upload-btn">Choisir une image</label>
    <input type="file" id="upload" accept="image/*" />

    <div class="controls">
        <label>
            Outil :
            <select id="toolSelector" style="padding: 5px; border-radius: var(--border-radius); border: 2px solid var(--primary-color);">
                <option value="fill">Pot de peinture (Clic)</option>
                <option value="brush">Pinceau (Glisser)</option>
            </select>
        </label>
        <label>
            Couleur :
            <input type="color" id="colorPicker" value="#ffc0cb" />
        </label>
        <label>
            Opacité (Pinceau) :
            <input type="range" id="opacitySlider" min="0" max="1" step="0.01" value="1" />
        </label>
        <label>
            Taille pinceau :
            <input type="range" id="brushSize" min="1" max="100" value="15" />
        </label>
        <button id="clear">Effacer tout</button>
        <button id="download">Télécharger l'image</button>
    </div>

    <canvas id="canvas"></canvas>

    <script>
        const upload = document.getElementById("upload");
        const canvas = document.getElementById("canvas");
        const ctx = canvas.getContext("2d");

        const colorPicker = document.getElementById("colorPicker");
        const opacitySlider = document.getElementById("opacitySlider");
        const brushSize = document.getElementById("brushSize");
        const clearBtn = document.getElementById("clear");
        const downloadBtn = document.getElementById("download");
        const toolSelector = document.getElementById("toolSelector");

        let drawing = false;
        let img = new Image();

        // Récupérer l'image sauvegardée
        const savedImage = localStorage.getItem("savedCanvas");
        if (savedImage) {
            const tempImg = new Image();
            tempImg.onload = function () {
                canvas.width = tempImg.width;
                canvas.height = tempImg.height;
                ctx.drawImage(tempImg, 0, 0);
            };
            tempImg.src = savedImage;
        }

        upload.addEventListener("change", (e) => {
            const file = e.target.files[0];
            const reader = new FileReader();

            reader.onload = function (event) {
                img = new Image();
                img.onload = function () {
                    canvas.width = img.width;
                    canvas.height = img.height;
                    ctx.drawImage(img, 0, 0);
                    saveCanvas();
                };
                img.src = event.target.result;
            };

            if (file) {
                reader.readAsDataURL(file);
            }
        });

        function saveCanvas() {
            const dataURL = canvas.toDataURL("image/png");
            localStorage.setItem("savedCanvas", dataURL);
        }

        function getCoordinates(e) {
            const rect = canvas.getBoundingClientRect();
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            let clientX = e.clientX;
            let clientY = e.clientY;
            
            if (e.touches && e.touches.length > 0) {
                clientX = e.touches[0].clientX;
                clientY = e.touches[0].clientY;
            }
            
            return {
                x: Math.floor((clientX - rect.left) * scaleX),
                y: Math.floor((clientY - rect.top) * scaleY)
            };
        }

        function drawAt(x, y) {
            const color = colorPicker.value;
            const opacity = parseFloat(opacitySlider.value);
            const size = parseInt(brushSize.value);

            const r = parseInt(color.slice(1, 3), 16);
            const g = parseInt(color.slice(3, 5), 16);
            const b = parseInt(color.slice(5, 7), 16);

            ctx.fillStyle = `rgba(${r}, ${g}, ${b}, ${opacity})`;
            ctx.beginPath();
            ctx.arc(x, y, size, 0, Math.PI * 2);
            ctx.fill();
        }

        function hexToRgb(hex) {
            const r = parseInt(hex.slice(1, 3), 16);
            const g = parseInt(hex.slice(3, 5), 16);
            const b = parseInt(hex.slice(5, 7), 16);
            return {r, g, b};
        }

        function floodFill(startX, startY, fillColorHex, tolerance = 80) {
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const data = imageData.data;
            const width = canvas.width;
            const height = canvas.height;
            
            if (startX < 0 || startX >= width || startY < 0 || startY >= height) return;

            const startPos = (startY * width + startX) * 4;
            const startR = data[startPos];
            const startG = data[startPos + 1];
            const startB = data[startPos + 2];
            const startA = data[startPos + 3];

            const fillRgb = hexToRgb(fillColorHex);
            const fillR = fillRgb.r;
            const fillG = fillRgb.g;
            const fillB = fillRgb.b;
            const fillA = 255;

            if (startR === fillR && startG === fillG && startB === fillB && startA === fillA) {
                return;
            }

            const visited = new Uint8Array(width * height);

            const matchColor = (pos) => {
                const r = data[pos];
                const g = data[pos + 1];
                const b = data[pos + 2];
                const a = data[pos + 3];
                return Math.abs(r - startR) <= tolerance &&
                       Math.abs(g - startG) <= tolerance &&
                       Math.abs(b - startB) <= tolerance &&
                       Math.abs(a - startA) <= tolerance;
            };

            const colorPixel = (pos) => {
                data[pos] = fillR;
                data[pos + 1] = fillG;
                data[pos + 2] = fillB;
                data[pos + 3] = fillA;
            };

            const stack = [[startX, startY]];
            
            while (stack.length > 0) {
                const [x, y] = stack.pop();
                const pixelIndex = y * width + x;
                
                if (visited[pixelIndex]) continue;
                
                let currentPos = pixelIndex * 4;
                
                let leftX = x;
                while (leftX >= 0 && matchColor(currentPos) && !visited[y * width + leftX]) {
                    leftX--;
                    currentPos -= 4;
                }
                leftX++;
                currentPos += 4;
                
                let rightX = leftX;
                let scanAbove = false;
                let scanBelow = false;
                
                while (rightX < width && matchColor(currentPos) && !visited[y * width + rightX]) {
                    colorPixel(currentPos);
                    visited[y * width + rightX] = 1;
                    
                    if (y > 0) {
                        const abovePos = currentPos - width * 4;
                        if (!visited[(y - 1) * width + rightX] && matchColor(abovePos)) {
                            if (!scanAbove) {
                                stack.push([rightX, y - 1]);
                                scanAbove = true;
                            }
                        } else {
                            scanAbove = false;
                        }
                    }
                    
                    if (y < height - 1) {
                        const belowPos = currentPos + width * 4;
                        if (!visited[(y + 1) * width + rightX] && matchColor(belowPos)) {
                            if (!scanBelow) {
                                stack.push([rightX, y + 1]);
                                scanBelow = true;
                            }
                        } else {
                            scanBelow = false;
                        }
                    }
                    
                    rightX++;
                    currentPos += 4;
                }
            }
            
            ctx.putImageData(imageData, 0, 0);
        }

        canvas.addEventListener("mousedown", (e) => {
            const coords = getCoordinates(e);
            if (toolSelector.value === 'fill') {
                floodFill(coords.x, coords.y, colorPicker.value, 80);
                saveCanvas();
            } else {
                drawing = true;
                drawAt(coords.x, coords.y);
            }
        });

        canvas.addEventListener("mousemove", (e) => {
            if (!drawing || toolSelector.value === 'fill') return;
            const coords = getCoordinates(e);
            drawAt(coords.x, coords.y);
        });

        canvas.addEventListener("mouseup", () => {
            if (drawing) {
                drawing = false;
                saveCanvas();
            }
        });

        canvas.addEventListener("mouseleave", () => {
            if (drawing) {
                drawing = false;
                saveCanvas();
            }
        });

        // Support mobile : touch
        canvas.addEventListener("touchstart", (e) => {
            if (e.touches.length === 1) e.preventDefault();
            const coords = getCoordinates(e);
            if (toolSelector.value === 'fill') {
                floodFill(coords.x, coords.y, colorPicker.value, 80);
                saveCanvas();
            } else {
                drawing = true;
                drawAt(coords.x, coords.y);
            }
        }, { passive: false });

        canvas.addEventListener("touchmove", (e) => {
            if (!drawing || toolSelector.value === 'fill') return;
            e.preventDefault();
            const coords = getCoordinates(e);
            drawAt(coords.x, coords.y);
        }, { passive: false });

        canvas.addEventListener("touchend", () => {
            if (drawing) {
                drawing = false;
                saveCanvas();
            }
        });

        clearBtn.addEventListener("click", () => {
            if (img.src) {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                ctx.drawImage(img, 0, 0);
            } else {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
            }
            saveCanvas();
        });

        downloadBtn.addEventListener("click", () => {
            const link = document.createElement("a");
            link.download = "image_coloriee.png";
            link.href = canvas.toDataURL("image/png");
            link.click();
        });
    </script>
</body>
</html>
