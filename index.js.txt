document.addEventListener('DOMContentLoaded', function() {
    const fileInput = document.getElementById('fileInput');
    const processButton = document.getElementById('processButton');
    const downloadLink = document.getElementById('downloadLink');

    processButton.addEventListener('click', function() {
        const files = fileInput.files;
        if (files.length === 0) {
            alert('Please select Excel files to process.');
            return;
        }

        // Create a FormData object and append selected files
        const formData = new FormData();
        for (let i = 0; i < files.length; i++) {
            formData.append('excelFiles', files[i]);
        }

        // Send a POST request to your server to process the files
        fetch('/process-excel', {
            method: 'POST',
            body: formData
        })
        .then(response => response.blob())
        .then(data => {
            // Display the download link
            downloadLink.href = URL.createObjectURL(data);
            downloadLink.style.display = 'block';
        })
        .catch(error => {
            console.error('An error occurred:', error);
        });
    });
});
