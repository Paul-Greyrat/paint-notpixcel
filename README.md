// Đảm bảo rằng canvas đã tồn tại trong DOM

document.addEventListener('DOMContentLoaded', function() {

    // Lấy tham chiếu đến thẻ canvas
    const canvas = document.getElementById('canvasHolder');
    
    // Kiểm tra nếu canvas tồn tại và có thể lấy ngữ cảnh 2D
    if (canvas && canvas.getContext) {
        const context = canvas.getContext('2d'); // Lấy ngữ cảnh 2D từ canvas

        // Lấy dữ liệu hình ảnh của toàn bộ canvas
        const imageData = context.getImageData(0, 0, canvas.width, canvas.height);
        const data = imageData.data;

        // Mã màu cam #ff9600 trong hệ RGB
        const targetColor = {r: 255, g: 150, b: 0};

        // Hàm để so sánh màu pixel với màu mục tiêu
        function isDifferentColor(r, g, b) {
            return !(r === targetColor.r && g === targetColor.g && b === targetColor.b);
        }

        // Hàm để click vào một điểm trên canvas
        function clickOnCanvas(x, y) {
            const event = new MouseEvent('click', {
                bubbles: true,
                cancelable: true,
                clientX: x,
                clientY: y
            });
            canvas.dispatchEvent(event); // Giả lập click chuột vào canvas
            console.log(`Đã click vào tọa độ (${x}, ${y}) với màu khác #ff9600`);
        }

        // Lặp qua từng pixel trên canvas
        for (let y = 0; y < canvas.height; y++) {
            for (let x = 0; x < canvas.width; x++) {
                // Tính chỉ số pixel trong mảng dữ liệu
                const index = (y * canvas.width + x) * 4;

                // Lấy giá trị RGB của pixel
                const r = data[index];
                const g = data[index + 1];
                const b = data[index + 2];

                // Kiểm tra nếu màu pixel khác màu cam #ff9600
                if (isDifferentColor(r, g, b)) {
                    clickOnCanvas(x, y); // Click vào tọa độ (x, y)
                    return; // Dừng lại sau khi tìm thấy và click vào pixel khác màu
                }
            }
        }

        console.log('Không tìm thấy màu khác màu #ff9600.');
    } else {
        console.error('Canvas không tồn tại hoặc không hỗ trợ ngữ cảnh 2D.');
    }
});
