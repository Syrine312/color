

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ColorApp</title>
    <!-- Modern typography -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;800&family=Cairo:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #6366f1; /* Indigo */
            --secondary-color: #ec4899; /* Pink */
            --accent-color: #8b5cf6; /* Purple */
            --light-color: #ffffff;
            --dark-color: #1f2937;
            --bg-color: #f3f4f6;
            --border-radius: 16px;
            --box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            --glass-bg: rgba(255, 255, 255, 0.7);
            --glass-border: 1px solid rgba(255, 255, 255, 0.5);
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #f2e8fc 0%, #e1edfa 100%);
            color: var(--dark-color);
            min-height: 100vh;
            transition: all 0.3s ease;
            box-sizing: border-box;
        }

        [dir="rtl"] {
            font-family: 'Cairo', sans-serif;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            max-width: 1000px;
            margin-bottom: 20px;
            background: var(--glass-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            padding: 15px 30px;
            border-radius: 100px;
            box-shadow: var(--box-shadow);
            border: var(--glass-border);
            box-sizing: border-box;
        }

        h2 {
            margin: 0;
            font-size: 2rem;
            font-weight: 800;
            background: linear-gradient(to right, var(--primary-color), var(--secondary-color));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-align: center;
            flex-grow: 1;
        }

        .lang-selector {
            padding: 8px 16px;
            border-radius: 20px;
            border: 1px solid #e5e7eb;
            background: rgba(255,255,255,0.9);
            cursor: pointer;
            font-family: inherit;
            font-weight: 600;
            font-size: 0.95rem;
            color: var(--dark-color);
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            outline: none;
            transition: all 0.2s ease;
        }

        .lang-selector:hover {
            border-color: var(--primary-color);
        }

        .spacer-for-flex {
            width: 100px; /* Balances the lang-selector to keep title centered */
        }

        canvas {
            border: none;
            border-radius: var(--border-radius);
            cursor: crosshair;
            margin-top: 10px;
            box-shadow: var(--box-shadow);
            background-color: white;
            max-width: 100%;
            transition: transform 0.3s ease;
        }

        .controls-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            width: 100%;
            max-width: 1000px;
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: var(--glass-border);
            padding: 25px;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            margin-bottom: 20px;
            box-sizing: border-box;
        }

        .controls-row {
            display: flex;
            gap: 25px;
            align-items: center;
            flex-wrap: wrap;
            justify-content: center;
            width: 100%;
        }

        label {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-weight: 600;
            color: var(--dark-color);
            gap: 10px;
            font-size: 0.95rem;
        }

        select, input[type="range"] {
            cursor: pointer;
        }
        
        select {
            padding: 10px 15px;
            border-radius: 12px;
            border: 1px solid #cbd5e1;
            background: var(--light-color);
            font-family: inherit;
            font-size: 0.95rem;
            outline: none;
            transition: all 0.2s ease;
        }
        
        select:hover {
            border-color: var(--primary-color);
        }

        input[type="color"] {
            width: 50px;
            height: 50px;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            padding: 0;
            background: none;
        }
        input[type="color"]::-webkit-color-swatch-wrapper {
            padding: 0;
        }
        input[type="color"]::-webkit-color-swatch {
            border: 3px solid #fff;
            border-radius: 50%;
            box-shadow: 0 4px 6px rgba(0,0,0,0.15);
        }

        button, .upload-btn {
            padding: 12px 28px;
            font-size: 1rem;
            cursor: pointer;
            background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
            color: white;
            border: none;
            border-radius: 30px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            font-weight: 600;
            box-shadow: 0 4px 10px rgba(99, 102, 241, 0.3);
            text-align: center;
            display: inline-block;
        }

        button:hover, .upload-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 15px -3px rgba(99, 102, 241, 0.4);
        }

        button:active, .upload-btn:active {
            transform: translateY(0);
        }

        #upload {
            display: none;
        }

        #clear {
            background: linear-gradient(135deg, #ef4444, #f43f5e);
            box-shadow: 0 4px 10px rgba(239, 68, 68, 0.3);
        }
        #clear:hover {
            box-shadow: 0 10px 15px -3px rgba(239, 68, 68, 0.4);
        }

        #download {
            background: linear-gradient(135deg, #10b981, #059669);
            box-shadow: 0 4px 10px rgba(16, 185, 129, 0.3);
        }
        #download:hover {
            box-shadow: 0 10px 15px -3px rgba(16, 185, 129, 0.4);
        }
        
        .action-buttons {
            display: flex;
            gap: 15px;
            margin-top: 5px;
            flex-wrap: wrap;
            justify-content: center;
            width: 100%;
        }

        @media (max-width: 768px) {
            .controls-row {
                flex-direction: column;
                align-items: stretch;
            }
            label {
                flex-direction: row;
                justify-content: space-between;
                width: 100%;
            }
            .action-buttons {
                flex-direction: column;
                width: 100%;
            }
            button, .upload-btn {
                width: 100%;
                box-sizing: border-box;
            }
            .header {
                flex-direction: column;
                gap: 15px;
                padding: 20px;
                border-radius: var(--border-radius);
            }
            .spacer-for-flex {
                display: none;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <select id="langSelector" class="lang-selector">
            <option value="fr">Français</option>
            <option value="en">English</option>
            <option value="ar">العربية</option>
        </select>
        <h2 id="titleText">Colorer maintenant</h2>
        <div class="spacer-for-flex"></div>
    </div>

    <div class="controls-container">
        <div class="controls-row">
            <label for="upload" class="upload-btn" id="uploadText">Choisir une image</label>
            <input type="file" id="upload" accept="image/*" />
        </div>

        <div class="controls-row">
            <label>
                <span id="toolLabel">Outil :</span>
                <select id="toolSelector">
                    <option value="fill" id="optFill">Pot de peinture (Clic)</option>
                    <option value="erase" id="optErase">Gomme magique (Clic)</option>
                    <option value="brush" id="optBrush">Pinceau (Glisser)</option>
                </select>
            </label>
            <label>
                <span id="colorLabel">Couleur :</span>
                <input type="color" id="colorPicker" value="#ec4899" />
            </label>
            <label>
                <span id="opacityLabel">Opacité (Pinceau) :</span>
                <input type="range" id="opacitySlider" min="0" max="1" step="0.01" value="1" />
            </label>
            <label>
                <span id="sizeLabel">Taille pinceau :</span>
                <input type="range" id="brushSize" min="1" max="100" value="15" />
            </label>
        </div>

        <div class="action-buttons">
            <button id="clear">Effacer tout</button>
            <button id="download">Télécharger l'image</button>
        </div>
    </div>

    <canvas id="canvas"></canvas>

    <script>
        const translations = {
            fr: {
                title: "Colorer Maintenant",
                upload: "Choisir une image",
                tool: "Outil :",
                toolFill: "Pot de peinture (Clic)",
                toolErase: "Gomme magique (Clic)",
                toolBrush: "Pinceau (Glisser)",
                color: "Couleur :",
                opacity: "Opacité (Pinceau) :",
                brushSize: "Taille pinceau :",
                clear: "Effacer tout",
                download: "Télécharger l'image"
            },
            en: {
                title: "Color Now",
                upload: "Choose an image",
                tool: "Tool:",
                toolFill: "Paint Bucket (Click)",
                toolErase: "Magic Eraser (Click)",
                toolBrush: "Brush (Drag)",
                color: "Color:",
                opacity: "Opacity (Brush):",
                brushSize: "Brush Size:",
                clear: "Clear All",
                download: "Download Image"
            },
            ar: {
                title: "لوّن الآن",
                upload: "اختر صورة",
                tool: "الأداة:",
                toolFill: "دلو الألوان (نقر)",
                toolErase: "ممحاة سحرية (نقر)",
                toolBrush: "فرشاة (سحب)",
                color: "اللون:",
                opacity: "الشفافية (الفرشاة):",
                brushSize: "حجم الفرشاة:",
                clear: "مسح الكل",
                download: "تنزيل الصورة"
            }
        };

        function updateLanguage(lang) {
            document.documentElement.lang = lang;
            document.documentElement.dir = lang === 'ar' ? 'rtl' : 'ltr';
            
            document.getElementById('titleText').innerText = translations[lang].title;
            document.getElementById('uploadText').innerText = translations[lang].upload;
            document.getElementById('toolLabel').innerText = translations[lang].tool;
            document.getElementById('optFill').innerText = translations[lang].toolFill;
            document.getElementById('optErase').innerText = translations[lang].toolErase;
            document.getElementById('optBrush').innerText = translations[lang].toolBrush;
            document.getElementById('colorLabel').innerText = translations[lang].color;
            document.getElementById('opacityLabel').innerText = translations[lang].opacity;
            document.getElementById('sizeLabel').innerText = translations[lang].brushSize;
            document.getElementById('clear').innerText = translations[lang].clear;
            document.getElementById('download').innerText = translations[lang].download;
        }

        document.getElementById('langSelector').addEventListener('change', (e) => {
            updateLanguage(e.target.value);
            localStorage.setItem("preferredLanguage", e.target.value);
        });

        const savedLang = localStorage.getItem("preferredLanguage") || 'fr';
        document.getElementById('langSelector').value = savedLang;
        updateLanguage(savedLang);

        const upload = document.getElementById("upload");
        const canvas = document.getElementById("canvas");
        const ctx = canvas.getContext("2d", { willReadFrequently: true });

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
            } else if (toolSelector.value === 'erase') {
                floodFill(coords.x, coords.y, '#ffffff', 80);
                saveCanvas();
            } else {
                drawing = true;
                drawAt(coords.x, coords.y);
            }
        });

        canvas.addEventListener("mousemove", (e) => {
            if (!drawing || toolSelector.value === 'fill' || toolSelector.value === 'erase') return;
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
            } else if (toolSelector.value === 'erase') {
                floodFill(coords.x, coords.y, '#ffffff', 80);
                saveCanvas();
            } else {
                drawing = true;
                drawAt(coords.x, coords.y);
            }
        }, { passive: false });

        canvas.addEventListener("touchmove", (e) => {
            if (!drawing || toolSelector.value === 'fill' || toolSelector.value === 'erase') return;
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
