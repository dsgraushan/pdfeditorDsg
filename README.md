<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF Master - Advanced Online PDF Editor</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.11.338/pdf.min.js"></script>
    <script src="https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #4361ee;
            --secondary-color: #3f37c9;
            --accent-color: #4895ef;
            --danger-color: #f72585;
            --success-color: #4cc9f0;
            --light-color: #f8f9fa;
            --dark-color: #212529;
            --gray-color: #6c757d;
            --border-radius: 8px;
            --box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f7fa;
            color: var(--dark-color);
            line-height: 1.6;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 20px;
        }

        header {
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
            padding: 15px 0;
            box-shadow: var(--box-shadow);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.5rem;
            font-weight: 700;
        }

        .logo i {
            font-size: 1.8rem;
        }

        .main-content {
            display: flex;
            gap: 20px;
            margin-top: 20px;
        }

        .sidebar {
            width: 250px;
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            padding: 15px;
            height: fit-content;
        }

        .sidebar-section {
            margin-bottom: 20px;
        }

        .sidebar-title {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 10px;
            color: var(--primary-color);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .tool-btn {
            display: flex;
            align-items: center;
            gap: 10px;
            width: 100%;
            padding: 10px;
            border: none;
            background: none;
            text-align: left;
            border-radius: var(--border-radius);
            cursor: pointer;
            transition: var(--transition);
            margin-bottom: 5px;
            font-size: 0.9rem;
        }

        .tool-btn:hover {
            background-color: rgba(67, 97, 238, 0.1);
        }

        .tool-btn.active {
            background-color: rgba(67, 97, 238, 0.2);
            font-weight: 600;
        }

        .pdf-viewer-container {
            flex: 1;
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            padding: 20px;
            min-height: 80vh;
        }

        .toolbar {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid #eee;
        }

        .btn {
            padding: 8px 15px;
            border-radius: var(--border-radius);
            border: none;
            background-color: var(--primary-color);
            color: white;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
            transition: var(--transition);
        }

        .btn:hover {
            background-color: var(--secondary-color);
            transform: translateY(-2px);
        }

        .btn-secondary {
            background-color: white;
            color: var(--primary-color);
            border: 1px solid var(--primary-color);
        }

        .btn-secondary:hover {
            background-color: rgba(67, 97, 238, 0.1);
        }

        .btn-danger {
            background-color: var(--danger-color);
        }

        .btn-danger:hover {
            background-color: #d1146b;
        }

        .btn-success {
            background-color: var(--success-color);
        }

        .btn-success:hover {
            background-color: #3ab0d6;
        }

        #pdf-viewer {
            width: 100%;
            height: 70vh;
            border: 1px solid #eee;
            border-radius: var(--border-radius);
            overflow: auto;
            background-color: #f0f2f5;
            position: relative;
        }

        .page-container {
            margin: 0 auto 20px;
            box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
            position: relative;
        }

        .page-controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 15px;
            margin-top: 15px;
        }

        .page-info {
            font-size: 0.9rem;
            color: var(--gray-color);
        }

        .zoom-controls {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-left: auto;
        }

        .zoom-btn {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            background-color: white;
            border: 1px solid #ddd;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
        }

        .zoom-btn:hover {
            background-color: #f0f0f0;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: white;
            padding: 25px;
            border-radius: var(--border-radius);
            width: 90%;
            max-width: 500px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            position: relative;
        }

        .close-modal {
            position: absolute;
            top: 15px;
            right: 15px;
            font-size: 1.5rem;
            cursor: pointer;
            color: var(--gray-color);
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }

        .form-control {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            font-size: 1rem;
        }

        .loading {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100%;
            font-size: 1.2rem;
            color: var(--gray-color);
        }

        .loading i {
            margin-right: 10px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .alert {
            padding: 15px;
            border-radius: var(--border-radius);
            margin-bottom: 20px;
            display: none;
        }

        .alert-success {
            background-color: rgba(76, 201, 240, 0.2);
            color: #0a7a96;
            border-left: 4px solid var(--success-color);
        }

        .alert-danger {
            background-color: rgba(247, 37, 133, 0.2);
            color: #b5175b;
            border-left: 4px solid var(--danger-color);
        }

        .file-drop-area {
            border: 2px dashed #ccc;
            border-radius: var(--border-radius);
            padding: 30px;
            text-align: center;
            margin-bottom: 20px;
            cursor: pointer;
            transition: var(--transition);
        }

        .file-drop-area:hover {
            border-color: var(--primary-color);
            background-color: rgba(67, 97, 238, 0.05);
        }

        .file-drop-area.active {
            border-color: var(--primary-color);
            background-color: rgba(67, 97, 238, 0.1);
        }

        @media (max-width: 992px) {
            .main-content {
                flex-direction: column;
            }

            .sidebar {
                width: 100%;
            }
        }

        @media (max-width: 768px) {
            .toolbar {
                justify-content: center;
            }

            .zoom-controls {
                margin-left: 0;
                justify-content: center;
                width: 100%;
                margin-top: 10px;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="container header-content">
            <div class="logo">
                <i class="fas fa-file-pdf"></i>
                <span>PDF Master</span>
            </div>
            <div>
                <button id="download-btn" class="btn btn-success">
                    <i class="fas fa-download"></i> Download PDF
                </button>
            </div>
        </div>
    </header>

    <div class="container">
        <div id="alert-box"></div>

        <div class="main-content">
            <div class="sidebar">
                <div class="sidebar-section">
                    <div class="sidebar-title">
                        <i class="fas fa-file-import"></i>
                        <span>File</span>
                    </div>
                    <input type="file" id="file-input" accept=".pdf" style="display: none;">
                    <button class="tool-btn" onclick="document.getElementById('file-input').click()">
                        <i class="fas fa-folder-open"></i> Open PDF
                    </button>
                    <button class="tool-btn" id="merge-pdf-btn">
                        <i class="fas fa-object-group"></i> Merge PDFs
                    </button>
                    <button class="tool-btn" id="split-pdf-btn">
                        <i class="fas fa-cut"></i> Split PDF
                    </button>
                </div>

                <div class="sidebar-section">
                    <div class="sidebar-title">
                        <i class="fas fa-edit"></i>
                        <span>Edit</span>
                    </div>
                    <button class="tool-btn" id="add-text-btn">
                        <i class="fas fa-font"></i> Add Text
                    </button>
                    <button class="tool-btn" id="add-image-btn">
                        <i class="fas fa-image"></i> Add Image
                    </button>
                    <button class="tool-btn" id="highlight-btn">
                        <i class="fas fa-highlighter"></i> Highlight
                    </button>
                    <button class="tool-btn" id="draw-btn">
                        <i class="fas fa-pencil-alt"></i> Draw
                    </button>
                    <button class="tool-btn" id="erase-btn">
                        <i class="fas fa-eraser"></i> Erase
                    </button>
                </div>

                <div class="sidebar-section">
                    <div class="sidebar-title">
                        <i class="fas fa-cog"></i>
                        <span>Tools</span>
                    </div>
                    <button class="tool-btn" id="rotate-btn">
                        <i class="fas fa-redo"></i> Rotate Page
                    </button>
                    <button class="tool-btn" id="delete-page-btn">
                        <i class="fas fa-trash"></i> Delete Page
                    </button>
                    <button class="tool-btn" id="reorder-pages-btn">
                        <i class="fas fa-sort-numeric-up"></i> Reorder Pages
                    </button>
                </div>
            </div>

            <div class="pdf-viewer-container">
                <div class="toolbar">
                    <button class="btn btn-secondary" id="prev-page">
                        <i class="fas fa-chevron-left"></i> Previous
                    </button>
                    <button class="btn btn-secondary" id="next-page">
                        Next <i class="fas fa-chevron-right"></i>
                    </button>
                    <div class="page-info">
                        Page <span id="page-num">1</span> of <span id="page-count">0</span>
                    </div>
                    <div class="zoom-controls">
                        <button class="zoom-btn" id="zoom-out">
                            <i class="fas fa-search-minus"></i>
                        </button>
                        <span id="zoom-level">100%</span>
                        <button class="zoom-btn" id="zoom-in">
                            <i class="fas fa-search-plus"></i>
                        </button>
                    </div>
                </div>

                <div id="pdf-viewer">
                    <div class="file-drop-area" id="file-drop-area">
                        <i class="fas fa-file-pdf" style="font-size: 3rem; color: var(--primary-color); margin-bottom: 15px;"></i>
                        <h3>Drag & Drop PDF File Here</h3>
                        <p>or click to browse files</p>
                    </div>
                </div>

                <div class="page-controls">
                    <button class="btn btn-secondary" id="first-page">
                        <i class="fas fa-step-backward"></i> First
                    </button>
                    <button class="btn btn-secondary" id="prev-page-bottom">
                        <i class="fas fa-chevron-left"></i> Previous
                    </button>
                    <button class="btn btn-secondary" id="next-page-bottom">
                        Next <i class="fas fa-chevron-right"></i>
                    </button>
                    <button class="btn btn-secondary" id="last-page">
                        Last <i class="fas fa-step-forward"></i>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Add Text Modal -->
    <div class="modal" id="text-modal">
        <div class="modal-content">
            <span class="close-modal">&times;</span>
            <h3>Add Text to PDF</h3>
            <div class="form-group">
                <label for="text-content">Text Content</label>
                <textarea id="text-content" class="form-control" rows="4"></textarea>
            </div>
            <div class="form-group">
                <label for="text-font">Font</label>
                <select id="text-font" class="form-control">
                    <option value="Helvetica">Helvetica</option>
                    <option value="Times-Roman">Times New Roman</option>
                    <option value="Courier">Courier</option>
                </select>
            </div>
            <div class="form-group">
                <label for="text-size">Size</label>
                <input type="number" id="text-size" class="form-control" value="12" min="8" max="72">
            </div>
            <div class="form-group">
                <label for="text-color">Color</label>
                <input type="color" id="text-color" class="form-control" value="#000000">
            </div>
            <button class="btn" id="add-text-confirm">
                <i class="fas fa-plus"></i> Add Text
            </button>
        </div>
    </div>

    <!-- Add Image Modal -->
    <div class="modal" id="image-modal">
        <div class="modal-content">
            <span class="close-modal">&times;</span>
            <h3>Add Image to PDF</h3>
            <div class="file-drop-area" id="image-drop-area">
                <i class="fas fa-image" style="font-size: 3rem; color: var(--primary-color); margin-bottom: 15px;"></i>
                <h3>Drag & Drop Image Here</h3>
                <p>or click to browse files</p>
                <input type="file" id="image-input" accept="image/*" style="display: none;">
            </div>
            <div class="form-group" id="image-preview-container" style="display: none;">
                <label>Image Preview</label>
                <div style="border: 1px solid #ddd; padding: 10px; text-align: center;">
                    <img id="image-preview" style="max-width: 100%; max-height: 200px;">
                </div>
            </div>
            <div class="form-group">
                <label for="image-width">Width (px)</label>
                <input type="number" id="image-width" class="form-control" value="200" min="50" max="1000">
            </div>
            <div class="form-group">
                <label for="image-height">Height (px)</label>
                <input type="number" id="image-height" class="form-control" value="200" min="50" max="1000">
            </div>
            <button class="btn" id="add-image-confirm" disabled>
                <i class="fas fa-plus"></i> Add Image
            </button>
        </div>
    </div>

    <script>
        // Set PDF.js worker path
        pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.11.338/pdf.worker.min.js';
        
        let pdfDoc = null,
            pageNum = 1,
            pageRendering = false,
            pageNumPending = null,
            scale = 1.0,
            currentTool = null,
            pdfBytes = null;

        const pdfViewer = document.getElementById('pdf-viewer');
        const pageNumElement = document.getElementById('page-num');
        const pageCountElement = document.getElementById('page-count');
        const fileInput = document.getElementById('file-input');
        const fileDropArea = document.getElementById('file-drop-area');
        const zoomLevelElement = document.getElementById('zoom-level');
        const alertBox = document.getElementById('alert-box');

        // Initialize modals
        const textModal = document.getElementById('text-modal');
        const imageModal = document.getElementById('image-modal');
        const closeModalButtons = document.querySelectorAll('.close-modal');

        // Show alert message
        function showAlert(message, type = 'success') {
            const alert = document.createElement('div');
            alert.className = `alert alert-${type}`;
            alert.innerHTML = message;
            alertBox.appendChild(alert);
            alert.style.display = 'block';
            
            setTimeout(() => {
                alert.style.opacity = '0';
                setTimeout(() => alert.remove(), 300);
            }, 3000);
        }

        // Close modal function
        function closeModal(modal) {
            modal.style.display = 'none';
        }

        // Set up event listeners for modals
        closeModalButtons.forEach(button => {
            button.addEventListener('click', function() {
                const modal = this.closest('.modal');
                closeModal(modal);
            });
        });

        // Close modal when clicking outside
        window.addEventListener('click', function(event) {
            if (event.target.classList.contains('modal')) {
                closeModal(event.target);
            }
        });

        // File drop functionality
        fileDropArea.addEventListener('click', () => fileInput.click());
        
        ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
            fileDropArea.addEventListener(eventName, preventDefaults, false);
        });

        function preventDefaults(e) {
            e.preventDefault();
            e.stopPropagation();
        }

        ['dragenter', 'dragover'].forEach(eventName => {
            fileDropArea.addEventListener(eventName, highlight, false);
        });

        ['dragleave', 'drop'].forEach(eventName => {
            fileDropArea.addEventListener(eventName, unhighlight, false);
        });

        function highlight() {
            fileDropArea.classList.add('active');
        }

        function unhighlight() {
            fileDropArea.classList.remove('active');
        }

        fileDropArea.addEventListener('drop', handleDrop, false);

        function handleDrop(e) {
            const dt = e.dataTransfer;
            const file = dt.files[0];
            
            if (file && file.type === 'application/pdf') {
                handleFile(file);
            } else {
                showAlert('Please upload a valid PDF file', 'danger');
            }
        }

        // Handle file input
        fileInput.addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file) handleFile(file);
        });

        function handleFile(file) {
            const fileReader = new FileReader();
            
            fileReader.onloadstart = function() {
                pdfViewer.innerHTML = '<div class="loading"><i class="fas fa-spinner"></i> Loading PDF...</div>';
            };
            
            fileReader.onload = function() {
                const typedArray = new Uint8Array(this.result);
                loadPDF(typedArray);
                pdfBytes = typedArray;
            };
            
            fileReader.onerror = function() {
                showAlert('Error reading file', 'danger');
                pdfViewer.innerHTML = '<div class="file-drop-area" id="file-drop-area">' +
                    '<i class="fas fa-file-pdf" style="font-size: 3rem; color: var(--primary-color); margin-bottom: 15px;"></i>' +
                    '<h3>Drag & Drop PDF File Here</h3><p>or click to browse files</p></div>';
            };
            
            fileReader.readAsArrayBuffer(file);
        }

        // Load PDF function
        function loadPDF(data) {
            pdfjsLib.getDocument(data).promise.then(function(pdfDoc_) {
                pdfDoc = pdfDoc_;
                pageCountElement.textContent = pdfDoc.numPages;
                
                // Reset page number and render first page
                pageNum = 1;
                renderPage(pageNum);
                
                fileDropArea.style.display = 'none';
            }).catch(function(error) {
                console.error('Error loading PDF:', error);
                showAlert('Error loading PDF. Please try another file.', 'danger');
                pdfViewer.innerHTML = '<div class="file-drop-area" id="file-drop-area">' +
                    '<i class="fas fa-file-pdf" style="font-size: 3rem; color: var(--primary-color); margin-bottom: 15px;"></i>' +
                    '<h3>Drag & Drop PDF File Here</h3><p>or click to browse files</p></div>';
            });
        }

        // Render PDF page
        function renderPage(num) {
            pageRendering = true;
            
            pdfViewer.innerHTML = '<div class="loading"><i class="fas fa-spinner"></i> Rendering page...</div>';
            
            pdfDoc.getPage(num).then(function(page) {
                const viewport = page.getViewport({ scale: scale });
                const canvas = document.createElement('canvas');
                const context = canvas.getContext('2d');
                canvas.height = viewport.height;
                canvas.width = viewport.width;
                
                // Clear PDF viewer and append canvas
                pdfViewer.innerHTML = '';
                
                const pageContainer = document.createElement('div');
                pageContainer.className = 'page-container';
                pageContainer.style.width = `${viewport.width}px`;
                pageContainer.style.height = `${viewport.height}px`;
                pageContainer.appendChild(canvas);
                pdfViewer.appendChild(pageContainer);
                
                const renderContext = {
                    canvasContext: context,
                    viewport: viewport
                };
                
                const renderTask = page.render(renderContext);
                
                renderTask.promise.then(function() {
                    pageRendering = false;
                    if (pageNumPending !== null) {
                        renderPage(pageNumPending);
                        pageNumPending = null;
                    }
                });
            }).catch(function(error) {
                console.error('Error rendering page:', error);
                pdfViewer.innerHTML = '<div class="alert alert-danger">Error rendering page. Please try again.</div>';
            });
            
            pageNumElement.textContent = num;
            zoomLevelElement.textContent = `${Math.round(scale * 100)}%`;
        }

        function queueRenderPage(num) {
            if (pageRendering) {
                pageNumPending = num;
            } else {
                renderPage(num);
            }
        }

        // Navigation functions
        function onPrevPage() {
            if (pageNum <= 1) return;
            pageNum--;
            queueRenderPage(pageNum);
        }

        function onNextPage() {
            if (pageNum >= pdfDoc.numPages) return;
            pageNum++;
            queueRenderPage(pageNum);
        }

        function onFirstPage() {
            if (pageNum === 1) return;
            pageNum = 1;
            queueRenderPage(pageNum);
        }

        function onLastPage() {
            if (pageNum === pdfDoc.numPages) return;
            pageNum = pdfDoc.numPages;
            queueRenderPage(pageNum);
        }

        // Zoom functions
        function zoomIn() {
            if (scale >= 3.0) return;
            scale += 0.1;
            queueRenderPage(pageNum);
        }

        function zoomOut() {
            if (scale <= 0.5) return;
            scale -= 0.1;
            queueRenderPage(pageNum);
        }

        // Set up event listeners
        document.getElementById('prev-page').addEventListener('click', onPrevPage);
        document.getElementById('next-page').addEventListener('click', onNextPage);
        document.getElementById('prev-page-bottom').addEventListener('click', onPrevPage);
        document.getElementById('next-page-bottom').addEventListener('click', onNextPage);
        document.getElementById('first-page').addEventListener('click', onFirstPage);
        document.getElementById('last-page').addEventListener('click', onLastPage);
        document.getElementById('zoom-in').addEventListener('click', zoomIn);
        document.getElementById('zoom-out').addEventListener('click', zoomOut);

        // Tool buttons
        document.getElementById('add-text-btn').addEventListener('click', function() {
            if (!pdfDoc) {
                showAlert('Please load a PDF first', 'danger');
                return;
            }
            textModal.style.display = 'flex';
        });

        document.getElementById('add-image-btn').addEventListener('click', function() {
            if (!pdfDoc) {
                showAlert('Please load a PDF first', 'danger');
                return;
            }
            imageModal.style.display = 'flex';
        });

        // Image modal functionality
        const imageInput = document.getElementById('image-input');
        const imagePreviewContainer = document.getElementById('image-preview-container');
        const imagePreview = document.getElementById('image-preview');
        const addImageConfirm = document.getElementById('add-image-confirm');

        document.getElementById('image-drop-area').addEventListener('click', () => imageInput.click());
        
        imageInput.addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file && file.type.match('image.*')) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    imagePreview.src = event.target.result;
                    imagePreviewContainer.style.display = 'block';
                    addImageConfirm.disabled = false;
                };
                reader.readAsDataURL(file);
            }
        });

        // Add text to PDF
        document.getElementById('add-text-confirm').addEventListener('click', async function() {
            const text = document.getElementById('text-content').value;
            const font = document.getElementById('text-font').value;
            const size = parseInt(document.getElementById('text-size').value);
            const color = document.getElementById('text-color').value;
            
            if (!text) {
                showAlert('Please enter text to add', 'danger');
                return;
            }

            try {
                const { PDFDocument, rgb } = PDFLib;
                
                const pdfDoc = await PDFDocument.load(pdfBytes);
                const pages = pdfDoc.getPages();
                const firstPage = pages[pageNum - 1];
                
                // Convert hex color to RGB
                const r = parseInt(color.substr(1, 2), 16) / 255;
                const g = parseInt(color.substr(3, 2), 16) / 255;
                const b = parseInt(color.substr(5, 2), 16) / 255;
                
                firstPage.drawText(text, {
                    x: 50,
                    y: firstPage.getHeight() - 50,
                    size: size,
                    font: await pdfDoc.embedFont(font),
                    color: rgb(r, g, b),
                });
                
                // Save the modified PDF
                pdfBytes = await pdfDoc.save();
                
                // Reload the PDF
                loadPDF(pdfBytes);
                
                showAlert('Text added successfully');
                closeModal(textModal);
            } catch (error) {
                console.error('Error adding text:', error);
                showAlert('Error adding text to PDF', 'danger');
            }
        });

        // Add image to PDF
        document.getElementById('add-image-confirm').addEventListener('click', async function() {
            const file = imageInput.files[0];
            const width = parseInt(document.getElementById('image-width').value);
            const height = parseInt(document.getElementById('image-height').value);
            
            if (!file) {
                showAlert('Please select an image first', 'danger');
                return;
            }

            try {
                const { PDFDocument } = PDFLib;
                
                const pdfDoc = await PDFDocument.load(pdfBytes);
                const pages = pdfDoc.getPages();
                const firstPage = pages[pageNum - 1];
                
                const imageBytes = await file.arrayBuffer();
                let image;
                
                if (file.type === 'image/jpeg') {
                    image = await pdfDoc.embedJpg(imageBytes);
                } else if (file.type === 'image/png') {
                    image = await pdfDoc.embedPng(imageBytes);
                } else {
                    showAlert('Only JPG and PNG images are supported', 'danger');
                    return;
                }
                
                firstPage.drawImage(image, {
                    x: 50,
                    y: firstPage.getHeight() - height - 50,
                    width: width,
                    height: height,
                });
                
                // Save the modified PDF
                pdfBytes = await pdfDoc.save();
                
                // Reload the PDF
                loadPDF(pdfBytes);
                
                showAlert('Image added successfully');
                closeModal(imageModal);
                
                // Reset image modal
                imagePreviewContainer.style.display = 'none';
                addImageConfirm.disabled = true;
                imageInput.value = '';
            } catch (error) {
                console.error('Error adding image:', error);
                showAlert('Error adding image to PDF', 'danger');
            }
        });

        // Download PDF
        document.getElementById('download-btn').addEventListener('click', function() {
            if (!pdfDoc) {
                showAlert('Please load a PDF first', 'danger');
                return;
            }
            
            downloadPDF(pdfBytes, 'edited.pdf');
            showAlert('PDF downloaded successfully');
        });

        function downloadPDF(data, filename) {
            const blob = new Blob([data], { type: 'application/pdf' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = filename;
            link.click();
        }

        // Keyboard navigation
        document.addEventListener('keydown', function(e) {
            if (!pdfDoc) return;
            
            switch(e.key) {
                case 'ArrowLeft':
                    onPrevPage();
                    break;
                case 'ArrowRight':
                    onNextPage();
                    break;
                case 'Home':
                    onFirstPage();
                    break;
                case 'End':
                    onLastPage();
                    break;
                case '+':
                    zoomIn();
                    break;
                case '-':
                    zoomOut();
                    break;
            }
        });
    </script>
</body>
</html>
