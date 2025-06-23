<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title id="page-title">Thegioididong.com</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #fdf8f4;
            color: #333;
            transition: background-image 0.5s ease-in-out, background-color 0.5s ease-in-out;
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }
        header {
            background-color: #FFD100;
            color: #333;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        #shop-name-display {
            color: #333;
        }
        .modal-overlay {
            background-color: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(8px);
            transition: opacity 0.3s ease-in-out;
            opacity: 0;
            pointer-events: none;
        }
        .modal-overlay.active {
            opacity: 1;
            pointer-events: auto;
        }
        .modal-content {
            transform: scale(0.9);
            transition: transform 0.3s ease-in-out;
            border-radius: 1.5rem;
        }
        .modal-overlay.active .modal-content {
            transform: scale(1);
        }
        .price-original {
            text-decoration: line-through;
            color: #999;
        }
        .price-discounted {
            color: #e53e3e;
            font-weight: 600;
        }
        .order-status-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: relative;
            margin-bottom: 2rem;
        }
        .order-status-step {
            flex: 1;
            text-align: center;
            padding: 0.4rem 0;
            border-bottom: 3px solid #ccc;
            color: #999;
            font-weight: 500;
            position: relative;
            transition: all 0.3s ease-in-out;
        }
        .order-status-step.active {
            border-color: #4CAF50;
            color: #4CAF50;
        }
        .order-status-step::after {
            content: '';
            position: absolute;
            bottom: -3px;
            left: 50%;
            transform: translateX(-50%);
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-bottom: 8px solid transparent;
            transition: all 0.3s ease-in-out;
        }
        .order-status-step.active::after {
            border-bottom-color: #4CAF50;
        }
        .tab-button.active {
            background-color: #4CAF50;
            color: white;
        }
        .option-button.selected {
            background-color: #d1e7dd;
            border-color: #28a745;
            color: #212529;
            position: relative;
        }
        .option-button.selected::after {
            content: "\f00c";
            font-family: "Font Awesome 5 Free";
            font-weight: 900;
            position: absolute;
            top: 5px;
            right: 5px;
            color: #28a745;
            font-size: 0.8em;
        }
        .blurred-content {
            filter: blur(5px);
            transition: filter 0.2s ease-in-out;
        }
        .masked-input-wrapper {
            position: relative;
        }
        .masked-input-wrapper .toggle-visibility-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            right: 0.5rem;
            padding: 0 0.75rem;
            display: flex;
            align-items: center;
            color: #6b7280;
            background: transparent;
            border: none;
            cursor: pointer;
        }
        .masked-input-wrapper textarea + .toggle-visibility-btn {
            top: 0.5rem;
            height: auto;
        }
        .out-of-stock {
            color: #ef4444;
            font-weight: 600;
        }
        .order-expandable-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease-out;
        }
        .order-expandable-content.expanded {
            max-height: 500px;
            transition: max-height 0.5s ease-in;
        }
        .relative-container {
            position: relative;
        }
        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(255, 255, 255, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 9999;
            font-size: 1.5rem;
            color: #333;
        }
        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #3498db;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        @keyframes neon-glow {
            0%, 100% {
                text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 15px #00f, 0 0 20px #00f, 0 0 25px #00f, 0 0 30px #00f, 0 0 35px #00f;
            }
            50% {
                text-shadow: 0 0 2px #fff, 0 0 5px #fff, 0 0 8px #00f, 0 0 12px #00f, 0 0 15px #00f, 0 0 20px #00f, 0 0 25px #00f;
            }
        }
        #shop-name-display.animated {
            animation: neon-glow 1.5s ease-in-out infinite alternate;
        }
        .advertisement-banner {
            background-color: #ffeb3b;
            color: #e53935;
            text-align: center;
            padding: 10px 0;
            font-weight: bold;
            font-size: 1.2rem;
            position: fixed;
            top: 48px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 45;
            overflow: hidden;
            border-radius: 0.5rem;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            height: 56px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .advertisement-banner .content {
            display: inline-block;
            white-space: nowrap;
            animation: marquee 15s linear infinite;
        }
        @keyframes marquee {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }
        @keyframes flicker {
            0%, 19%, 21%, 23%, 25%, 54%, 56%, 100% {
                opacity: 1;
                text-shadow: 0 0 4px #ff0, 0 0 10px #ff0, 0 0 20px #ff0, 0 0 40px #ff0;
            }
            20%, 24%, 55% {
                opacity: 0.5;
                text-shadow: none;
            }
        }
        .advertisement-banner.flicker .content {
            animation: flicker 2s infinite alternate, marquee 15s linear infinite;
        }
        .header-search-input {
            border-radius: 9999px;
            border: 1px solid #ccc;
            padding: 0.5rem 0.5rem 0.5rem 2.5rem;
            width: 300px;
            transition: width 0.3s ease-in-out;
        }
        .header-search-icon {
            position: absolute;
            left: 0.75rem;
            color: #6b7280;
        }
        @media (max-width: 768px) {
            .header-search-input {
                width: 100%;
            }
        }
        .star-rating .fa-star {
            color: gold;
        }
    </style>
</head>
<body class="bg-fdf8f4 text-333">

    <header class="fixed top-0 left-0 right-0 z-40 bg-yellow-400 text-gray-800 shadow-lg py-2 px-6 md:px-10 lg:px-16 w-full flex items-center justify-between h-12">
        <div class="flex items-center">
            <h1 id="shop-name-display" class="text-2xl lg:text-3xl font-bold rounded-full py-1 px-4 transition-all duration-300 ease-in-out hover:scale-105 cursor-pointer mr-4 lg:mr-6">
                <i class="fas fa-mobile-alt mr-2 text-gray-700"></i><span class="text-gray-900">Thegioididong</span><span class="text-red-600">.com</span>
            </h1>
            <div class="relative flex items-center w-48 md:w-64 lg:w-80">
                <input type="text" id="header-product-search-input" placeholder="Bạn tìm gì..." class="header-search-input focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm">
                <i class="fas fa-search header-search-icon"></i>
            </div>
        </div>

        <div class="flex items-center space-x-4 lg:space-x-6">
            <div id="cart-section" class="flex items-center">
                <button id="open-cart-modal-btn" class="flex items-center text-gray-800 hover:text-gray-900 font-medium py-1 px-2 rounded-full transition-all duration-200 relative text-sm">
                    <i class="fas fa-shopping-cart text-lg mr-2"></i>
                    Giỏ hàng
                    <span id="cart-count" class="absolute -top-1 -right-1 bg-red-600 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center">0</span>
                </button>
            </div>

            <div id="shop-address-container" class="flex items-center text-xs text-gray-800 text-right">
                <i class="fas fa-map-marker-alt text-base mr-1"></i>
                <div class="flex flex-col">
                    <span class="font-semibold mb-0.5">Địa chỉ cửa hàng:</span>
                    <span id="shop-address-display" class="text-xs italic text-gray-700">Chưa cập nhật</span>
                </div>
                <button id="open-address-in-map-btn" class="ml-2 bg-gray-200 hover:bg-gray-300 text-gray-800 text-xs py-1 px-2 rounded-full transition-all duration-200">
                    <i class="fas fa-external-link-alt mr-1"></i>
                </button>
            </div>
        </div>
    </header>

    <div id="advertisement-banner-container" class="advertisement-banner hidden fixed top-15 left-190 right-0 z-30 mx-auto w-[95%] md:w-[85%] lg:w-[80%] xl:w-[70%] p-3 rounded-lg h-14">
        <div class="content" id="advertisement-content"></div>
    </div>


    <div class="flex flex-1 w-full pt-28">
        <aside class="fixed top-0 left-0 z-30 w-56 lg:w-64 h-full pt-20 bg-white p-4 rounded-xl shadow-lg overflow-y-auto border-r border-gray-200">
            <h2 class="text-xl font-semibold mb-6 text-gray-800">MENU</h2>
            <nav>
                <ul class="space-y-3"> 
                    <li><button id="show-products-btn" class="tab-button w-full text-left p-3 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-100 active"><i class="fas fa-boxes mr-2"></i>Gian Hàng</button></li>
                    <li><button id="show-created-orders-btn" class="tab-button w-full text-left p-3 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-100"><i class="fas fa-clipboard-list mr-2"></i>Đơn Hàng Đã Tạo</button></li>
                    <li><button id="show-shipping-orders-btn" class="tab-button w-full text-left p-3 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-100"><i class="fas fa-truck mr-2"></i>Đơn Hàng Đang Vận Chuyển</button></li>
                    <li><button id="show-delivered-orders-btn" class="tab-button w-full text-left p-3 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-100"><i class="fas fa-check-circle mr-2"></i>Đơn Hàng Đã Giao</button></li>
                    <li><button id="open-management-modal-btn" class="tab-button w-full text-left p-3 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-100"><i class="fas fa-cogs mr-2"></i>Nhập Hàng</button></li>
                    <li><button id="open-settings-modal-btn" class="tab-button w-full text-left p-3 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-100"><i class="fas fa-cog mr-2"></i>Cài Đặt</button></li>
                    <li><button id="open-shop-analytics-modal-btn" class="tab-button w-full text-left p-3 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-100"><i class="fas fa-chart-line mr-2"></i>Thống Kê</button></li>
                </ul>
            </nav>
        </aside>

        <section class="flex-1 ml-56 lg:ml-64 p-6 rounded-xl shadow-lg bg-white overflow-y-auto"> 
            <div id="product-list-section" class="active-section">
                <h2 class="text-3xl font-semibold mb-6 text-gray-800">Sản Phẩm Nổi Bật</h2>
                <div id="product-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                </div>
            </div>

            <div id="created-orders-section" class="hidden">
                <h2 class="text-3xl font-semibold mb-6 text-gray-800">Đơn Hàng Đã Tạo</h2>
                <div class="mb-4 flex items-center gap-2">
                    <input type="text" id="search-created-orders" placeholder="Tìm kiếm đơn hàng đã tạo..." class="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <button id="clear-search-created-orders" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-lg transition-all duration-200"><i class="fas fa-times"></i></button>
                </div>
                <div id="created-orders-list" class="space-y-4">
                </div>
            </div>

            <div id="shipping-orders-section" class="hidden">
                <h2 class="text-3xl font-semibold mb-6 text-gray-800">Đơn Hàng Đang Vận Chuyển</h2>
                   <div class="mb-4 flex items-center gap-2">
                    <input type="text" id="search-shipping-orders" placeholder="Tìm kiếm đơn hàng đang vận chuyển..." class="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <button id="clear-search-shipping-orders" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-lg transition-all duration-200"><i class="fas fa-times"></i></button>
                </div>
                <div id="shipping-orders-list" class="space-y-4">
                </div>
            </div>

            <div id="delivered-orders-section" class="hidden">
                <h2 class="text-3xl font-semibold mb-6 text-gray-800">Đơn Hàng Đã Giao</h2>
                <div class="mb-4 flex items-center gap-2">
                    <input type="text" id="search-delivered-orders" placeholder="Tìm kiếm đơn hàng đã giao..." class="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <button id="clear-search-delivered-orders" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-lg transition-all duration-200"><i class="fas fa-times"></i></button>
                </div>
                <div id="delivered-orders-list" class="space-y-4">
                </div>
            </div>
        </section>
    </div>

    <div id="product-detail-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-4xl w-full flex flex-col md:flex-row gap-8 relative modal-content max-h-[85vh] overflow-y-auto">
            <button id="close-product-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <div class="md:w-1/2 flex items-center justify-center p-4">
                <img id="modal-product-image" src="" alt="Sản phẩm" class="max-h-96 w-auto object-contain mb-4 rounded-lg shadow-md">
            </div>
            <div class="md:w-1/2">
                <h3 id="modal-product-name" class="text-3xl font-bold mb-4 text-gray-900"></h3>
                <p class="text-xl font-semibold mb-2 text-gray-700">Giá cơ bản: <span id="modal-product-base-price" class="font-bold"></span></p>
                <p class="text-xl font-semibold mb-2 text-gray-700">Tổng giá: <span id="modal-product-price-display" class="price-original"></span></p>
                <p id="modal-product-discount-display" class="text-xl font-semibold mb-4 price-discounted hidden"></p>
                
                <div id="product-options" class="mb-6 space-y-4">
                </div>
                
                <p class="text-gray-600 mb-2">Số lượng đã bán: <span id="modal-product-sold">N/A</span></p>
                <p class="text-gray-600 mb-4">Số lượng còn lại: <span id="modal-product-remaining">N/A</span></p>
                <p id="modal-product-description" class="text-gray-700 mb-4 text-sm"></p>
                
                <div class="mb-4 text-gray-700 text-sm">
                    <p class="mb-1"><i class="fas fa-undo-alt mr-2 text-blue-500"></i> Chính sách đổi trả: <span class="font-semibold">15 Ngày</span></p>
                    <p id="modal-shipping-cost-display"><i class="fas fa-shipping-fast mr-2 text-green-500"></i> Phí vận chuyển: <span class="font-semibold">28000đ</span></p>
                </div>

                <div class="mb-6">
                    <label for="voucher-code" class="block text-gray-700 text-sm font-bold mb-2">Mã Voucher:</label>
                    <div class="flex">
                        <input type="text" id="voucher-code" class="shadow appearance-none border rounded-l-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Nhập mã voucher">
                        <button id="apply-voucher-btn" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-r-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all duration-200"><i class="fas fa-ticket-alt"></i> Áp Dụng</button>
                    </div>
                </div>

                <div class="flex gap-4 mt-6">
                    <button id="buy-now-detail-btn" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-3 px-6 rounded-lg flex-1 transition-all duration-200 shadow-lg"><i class="fas fa-dollar-sign mr-2"></i>Mua Ngay</button>
                    <button id="add-to-cart-detail-btn" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-3 px-6 rounded-lg flex-1 transition-all duration-200 shadow-lg"><i class="fas fa-cart-plus mr-2"></i>Thêm vào giỏ hàng</button>
                </div>
            </div>
        </div>
    </div>

    <div id="order-creation-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-2xl w-full relative modal-content max-h-[85vh] overflow-y-auto">
            <button id="close-order-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <h3 class="text-3xl font-bold mb-6 text-gray-900 text-center">Tạo Đơn Hàng Mới</h3>

            <div class="mb-6">
                <p class="text-gray-700 text-lg mb-2">Mã đơn hàng: <span id="order-id-display" class="font-semibold text-blue-700"></span></p>
                <div id="order-products-summary" class="border p-4 rounded-lg bg-gray-50 max-h-40 overflow-y-auto">
                </div>
            </div>

            <form id="order-form" class="space-y-4">
                <div>
                    <label for="customer-name" class="block text-gray-700 text-lg font-bold mb-2">Họ tên:</label>
                    <input type="text" id="customer-name" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                </div>
                <div>
                    <label for="customer-phone" class="block text-gray-700 text-lg font-bold mb-2">Số điện thoại:</label>
                    <div class="masked-input-wrapper">
                        <input type="tel" id="customer-phone" class="shadow appearance-none border rounded-lg w-full py-3 px-4 pr-12 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                        <button type="button" class="toggle-visibility-btn" data-target-id="customer-phone">
                            <i class="fas fa-eye"></i>
                        </button>
                    </div>
                </div>
                <div>
                    <label for="customer-address" class="block text-gray-700 text-lg font-bold mb-2">Địa chỉ:</label>
                    <div class="masked-input-wrapper">
                        <textarea id="customer-address" class="shadow appearance-none border rounded-lg w-full py-3 px-4 pr-12 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500 h-24 resize-none" required></textarea>
                        <button type="button" class="toggle-visibility-btn" data-target-id="customer-address">
                            <i class="fas fa-eye"></i>
                        </button>
                    </div>
                </div>

                <div>
                    <label for="order-location" class="block text-gray-700 text-lg font-bold mb-2">Vị trí kho hàng :</label>
                    <input type="text" id="order-location" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Chi nhánh kho">
                </div>
                <div>
                    <label for="estimated-delivery-date" class="block text-gray-700 text-lg font-bold mb-2">Ngày dự kiến nhận hàng:</label>
                    <input type="date" id="estimated-delivery-date" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>

                <div class="order-status-bar">
                    <div class="order-status-step active" data-status="created">Tạo Đơn Hàng</div>
                    <div class="order-status-step" data-status="shipping">Đang Vận Chuyển</div>
                    <div class="order-status-step" data-status="delivered">Đã Giao Hàng</div>
                </div>

                <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg w-full transition-all duration-200 shadow-lg"><i class="fas fa-check-square mr-2"></i>Xác Nhận Đơn Hàng</button>
            </form>
        </div>
    </div>

    <div id="edit-shipping-order-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-xl w-full relative modal-content max-h-[85vh] overflow-y-auto">
            <button id="close-edit-shipping-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <h3 class="text-3xl font-bold mb-6 text-gray-900 text-center">Chỉnh Sửa Thông Tin Vận Chuyển</h3>
            <p class="text-gray-700 text-lg mb-4">Đơn hàng: <span id="edit-shipping-order-id-display" class="font-semibold text-blue-700"></span></p>

            <form id="edit-shipping-order-form" class="space-y-4">
                <input type="hidden" id="edit-shipping-order-hidden-id">
                <div>
                    <label for="edit-order-location" class="block text-gray-700 text-lg font-bold mb-2">Vị trí hiện tại:</label>
                    <input type="text" id="edit-order-location" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Ví dụ: Đang ở kho Hồ Chí Minh">
                </div>
                <div>
                    <label for="edit-estimated-delivery-date" class="block text-gray-700 text-lg font-bold mb-2">Ngày dự kiến nhận hàng:</label>
                    <input type="date" id="edit-estimated-delivery-date" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg w-full transition-all duration-200 shadow-lg"><i class="fas fa-save mr-2"></i>Lưu Thay Đổi</button>
            </form>
        </div>
    </div>

    <div id="cart-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-3xl w-full relative modal-content max-h-[85vh] overflow-y-auto">
            <button id="close-cart-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <h3 class="text-3xl font-bold mb-6 text-gray-900 text-center">Giỏ Hàng Của Bạn</h3>

            <div class="mb-4 flex items-center gap-2">
                <input type="text" id="search-cart-items" placeholder="Tìm kiếm sản phẩm trong giỏ hàng..." class="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                <button id="clear-search-cart-items" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-lg transition-all duration-200"><i class="fas fa-times"></i></button>
            </div>
            
            <div id="cart-items-list" class="space-y-4 mb-6">
                <p class="text-gray-500 italic text-center">Giỏ hàng trống.</p>
            </div>

            <div class="flex justify-between items-center border-t pt-4 border-gray-200">
                <span class="text-xl font-bold text-gray-900">Tổng cộng: <span id="cart-total-amount">0 VND</span></span>
                <button id="buy-all-cart-btn" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg transition-all duration-200 shadow-lg"><i class="fas fa-credit-card mr-2"></i>Mua Ngay Tất Cả</button>
            </div>
        </div>
    </div>


    <div id="shop-management-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-4xl w-full relative overflow-y-auto max-h-[90vh] modal-content">
            <button id="close-management-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <h3 class="text-3xl font-bold mb-6 text-gray-900 text-center">Cửa Hàng</h3>

            <div id="management-content-area" class="relative-container rounded-3xl p-4">
                <div class="mb-8 border-b pb-6 border-gray-200">
                    <h4 class="text-2xl font-semibold mb-4 text-gray-800">Quản lý Sản phẩm Hiện Có</h4>
                    <div id="product-management-list" class="space-y-4">
                        <p class="text-gray-500 italic">Đang tải danh sách sản phẩm...</p>
                    </div>
                </div>


                <div class="mb-8 border-b pb-6 border-gray-200">
                    <h4 id="add-edit-product-title" class="text-2xl font-semibold mb-4 text-gray-800">Thêm Mặt Hàng Mới</h4>
                    <form id="add-edit-product-form" class="space-y-4">
                        <input type="hidden" id="edit-product-id">
                        <div>
                            <label for="new-product-name" class="block text-gray-700 text-lg font-bold mb-2">Tên Sản Phẩm:</label>
                            <input type="text" id="new-product-name" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                        </div>
                        <div>
                            <label for="new-product-base-price" class="block text-gray-700 text-lg font-bold mb-2">Giá Cơ Bản (VND):</label>
                            <input type="number" id="new-product-base-price" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" required min="0">
                        </div>
                        
                        <div>
                            <label for="new-product-image" class="block text-gray-700 text-lg font-bold mb-2">URL Hình Ảnh Chính (URL trực tiếp, bao gồm GitHub Raw hoặc Base64):</label>
                            <div class="flex gap-2">
                                <input type="url" id="new-product-image" class="shadow appearance-none border rounded-l-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="https://example.com/image.jpg hoặc chuỗi Base64 dài">
                                <button type="button" id="upload-main-image-btn" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-r-lg transition-all duration-200 text-sm flex-shrink-0"><i class="fas fa-upload mr-2"></i>Tải ảnh lên</button>
                            </div>
                        </div>
                        <div>
                            <label for="new-product-description" class="block text-gray-700 text-lg font-bold mb-2">Mô tả sản phẩm:</label>
                            <textarea id="new-product-description" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500 h-20 resize-none"></textarea>
                        </div>
                        <div>
                            <label for="new-product-reviews" class="block text-gray-700 text-lg font-bold mb-2">Lượt Đánh Giá (Số lượng):</label>
                            <input type="number" id="new-product-reviews" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" value="0" min="0">
                        </div>

                        <div class="border p-4 rounded-lg bg-gray-50">
                            <h5 class="text-xl font-semibold mb-3 text-gray-700">Tùy Chọn Màu Sắc (kèm Giá tác động)</h5>
                            <div id="color-options-container" class="space-y-3">
                            </div>
                            <button type="button" id="add-color-option-btn" class="mt-4 bg-purple-500 hover:bg-purple-600 text-white font-bold py-2 px-4 rounded-lg transition-all duration-200"><i class="fas fa-plus mr-2"></i>Thêm Màu Sắc</button>
                        </div>

                        <div class="border p-4 rounded-lg bg-gray-50">
                            <h5 class="text-xl font-semibold mb-3 text-gray-700">Tùy Chọn Dung Lượng (kèm Giá tác động)</h5>
                            <div id="storage-options-container" class="space-y-3">
                            </div>
                            <button type="button" id="add-storage-option-btn" class="mt-4 bg-purple-500 hover:bg-purple-600 text-white font-bold py-2 px-4 rounded-lg transition-all duration-200"><i class="fas fa-plus mr-2"></i>Thêm Dung Lượng</button>
                        </div>

                        <div class="border p-4 rounded-lg bg-yellow-50">
                            <h5 class="text-xl font-semibold mb-3 text-gray-700">Các Biến Thể Sản Phẩm (Màu sắc + Dung lượng, kèm số lượng & Giá tác động riêng)</h5>
                            <div id="variants-container" class="space-y-3">
                            </div>
                            <button type="button" id="add-variant-btn" class="mt-4 bg-orange-500 hover:bg-orange-600 text-white font-bold py-2 px-4 rounded-lg transition-all duration-200"><i class="fas fa-plus mr-2"></i>Thêm Biến Thể</button>
                            <p class="text-sm text-gray-600 mt-2">Lưu ý: Chỉ thêm biến thể sau khi đã định nghĩa các Tùy chọn Màu sắc và Dung lượng phía trên.</p>
                        </div>

                        <button type="submit" id="submit-product-btn" class="bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-lg w-full transition-all duration-200 shadow-lg mt-6"><i class="fas fa-save mr-2"></i>Thêm Sản Phẩm</button>
                        <button type="button" id="cancel-edit-btn" class="hidden bg-gray-500 hover:bg-gray-600 text-white font-bold py-3 px-6 rounded-lg w-full transition-all duration-200 shadow-lg mt-2"><i class="fas fa-times-circle mr-2"></i>Hủy Chỉnh Sửa</button>
                    </form>
                </div>

                <div class="mt-8">
                    <h4 class="text-2xl font-semibold mb-4 text-gray-800">Thêm Mã Voucher Mới</h4>
                    <form id="add-voucher-form" class="space-y-4">
                        <div>
                            <label for="new-voucher-code" class="block text-gray-700 text-lg font-bold mb-2">Mã Voucher:</label>
                            <input type="text" id="new-voucher-code" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                        </div>
                        <div>
                            <label for="new-voucher-value" class="block text-gray-700 text-lg font-bold mb-2">Giá Trị Voucher (ví dụ: 0.10 cho 10%, 50000 cho 50.000 VND):</label>
                            <input type="number" step="any" id="new-voucher-value" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" required min="0">
                        </div>
                        <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg w-full transition-all duration-200 shadow-lg"><i class="fas fa-plus-square mr-2"></i>Thêm Voucher</button>
                    </form>

                    <div class="mt-8">
                        <h4 class="text-2xl font-semibold mb-4 text-gray-800">Voucher Hiện Có</h4>
                        <div id="current-vouchers-list" class="space-y-2">
                            <p class="text-gray-500 italic">Chưa có voucher nào.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="shop-settings-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-xl w-full relative modal-content max-h-[85vh] overflow-y-auto">
            <button id="close-settings-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <h3 class="text-3xl font-bold mb-6 text-gray-900 text-center">Cài Đặt Cửa Hàng</h3>
            
            <div id="settings-content-area" class="relative-container rounded-3xl p-4">
                <form id="shop-settings-form" class="space-y-4">
                    <div>
                        <label for="shop-name-input" class="block text-gray-700 text-lg font-bold mb-2">Tên Cửa Hàng:</label>
                        <input type="text" id="shop-name-input" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                    </div>
                    <div>
                        <label for="shop-address-input" class="block text-gray-700 text-lg font-bold mb-2">Địa chỉ Cửa Hàng:</label>
                        <input type="text" id="shop-address-input" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Nhập địa chỉ cửa hàng của bạn">
                    </div>
                    <div>
                        <label for="background-image-url" class="block text-gray-700 text-lg font-bold mb-2">URL Hình Nền:</label>
                        <div class="flex gap-2">
                            <input type="url" id="background-image-url" class="shadow appearance-none border rounded-l-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="https://example.com/image.jpg">
                                <button type="button" id="upload-background-btn" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-r-lg transition-all duration-200 text-sm flex-shrink-0"><i class="fas fa-upload mr-2"></i>Tải ảnh lên</button>
                            </div>
                        <small class="text-gray-500">Để trống nếu không muốn dùng hình nền.</small>
                    </div>
                    <div>
                        <label for="advertisement-text-input" class="block text-gray-700 text-lg font-bold mb-2">Nội dung quảng cáo:</label>
                        <input type="text" id="advertisement-text-input" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Ví dụ: Giảm giá 50% tất cả sản phẩm!">
                        <small class="text-gray-500">Để trống để ẩn banner quảng cáo.</small>
                    </div>
                    <div>
                        <label for="advertisement-animation-select" class="block text-gray-700 text-lg font-bold mb-2">Hiệu ứng quảng cáo:</label>
                        <select id="advertisement-animation-select" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="none">Không có</option>
                            <option value="marquee">Chữ chạy</option>
                            <option value="flicker">Nhấp nháy</option>
                        </select>
                    </div>

                    <div class="border p-4 rounded-lg bg-gray-50 space-y-4">
                        <h4 class="text-xl font-semibold mb-3 text-gray-700">Cài đặt Thanh toán QR Code Ngân hàng</h4>
                        <div>
                            <label for="bank-name-input" class="block text-gray-700 text-lg font-bold mb-2">Tên Ngân hàng:</label>
                            <input type="text" id="bank-name-input" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Ví dụ: Vietcombank">
                        </div>
                        <div>
                            <label for="account-number-input" class="block text-gray-700 text-lg font-bold mb-2">Số Tài khoản:</label>
                            <input type="text" id="account-number-input" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Ví dụ: 0123456789">
                        </div>
                        <div>
                            <label for="account-holder-input" class="block text-gray-700 text-lg font-bold mb-2">Tên Chủ tài khoản:</label>
                            <input type="text" id="account-holder-input" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Ví dụ: NGUYEN VAN A">
                        </div>
                        <div>
                            <label for="qr-code-image-url-input" class="block text-gray-700 text-lg font-bold mb-2">URL Ảnh QR Code Ngân hàng:</label>
                            <div class="flex gap-2">
                                <input type="url" id="qr-code-image-url-input" class="shadow appearance-none border rounded-l-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="https://example.com/qr_code.png">
                                <button type="button" id="upload-qr-code-btn" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-r-lg transition-all duration-200 text-sm flex-shrink-0"><i class="fas fa-upload mr-2"></i>Tải ảnh lên</button>
                            </div>
                            <small class="text-gray-500">Đây là ảnh QR Code mà khách hàng sẽ quét để thanh toán.</small>
                        </div>
                    </div>

                    <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg w-full transition-all duration-200 shadow-lg"><i class="fas fa-save mr-2"></i>Lưu Cài Đặt</button>
                </form>
            </div>
        </div>
    </div>

    <div id="shop-analytics-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-3xl w-full relative modal-content max-h-[85vh] overflow-y-auto">
            <button id="close-analytics-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <h3 class="text-3xl font-bold mb-6 text-gray-900 text-center">Thống Kê Cửa Hàng</h3>

            <div id="analytics-content-area" class="relative-container rounded-3xl p-4">
                <div class="mb-6 flex flex-col md:flex-row items-center gap-4">
                    <div class="flex-1 w-full">
                        <label for="report-start-date" class="block text-gray-700 text-sm font-bold mb-2">Từ ngày:</label>
                        <input type="date" id="report-start-date" class="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div class="flex-1 w-full">
                        <label for="report-end-date" class="block text-gray-700 text-sm font-bold mb-2">Đến ngày:</label>
                        <input type="date" id="report-end-date" class="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <button id="generate-report-btn" class="mt-6 md:mt-0 bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg transition-all duration-200 flex-shrink-0"><i class="fas fa-chart-bar mr-2"></i>Tạo Báo Cáo</button>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
                    <div class="bg-white p-6 rounded-lg shadow-md">
                        <h4 class="text-xl font-semibold text-gray-800 mb-4">Tổng Doanh Thu</h4>
                        <p id="total-revenue-display" class="text-3xl font-bold text-green-600">0 VND</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-md">
                        <h4 class="text-xl font-semibold text-gray-800 mb-4">Số Lượng Đơn Hàng</h4>
                        <p id="total-orders-display" class="text-3xl font-bold text-purple-600">0</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-md col-span-1 md:col-span-2">
                        <h4 class="text-xl font-semibold text-gray-800 mb-4">Sản Phẩm Bán Chạy Nhất</h4>
                        <ul id="top-selling-products-list" class="list-disc pl-5 text-gray-700">
                            <li class="italic text-gray-500">Chưa có dữ liệu.</li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="payment-voucher-modal" class="hidden fixed inset-0 flex items-center justify-center z-50 modal-overlay p-4">
        <div class="bg-blue-50 rounded-3xl shadow-2xl p-8 max-w-lg w-full relative modal-content max-h-[85vh] overflow-y-auto">
            <button id="close-payment-voucher-modal" class="absolute top-4 right-4 text-gray-600 hover:text-gray-800 text-3xl font-bold">&times;</button>
            <h3 class="text-3xl font-bold mb-6 text-gray-900 text-center">Thanh Toán Voucher (VAT)</h3>

            <div id="qr-payment-details" class="flex flex-col items-center mb-6">
                <img id="qr-code-display" src="" onerror="this.onerror=null;this.src='https://placehold.co/200x200/cccccc/333333?text=QR+Code';" alt="Mã QR Ngân hàng" class="w-48 h-48 object-contain rounded-lg mb-4 shadow-md">
                <p class="text-gray-700 text-lg font-semibold"><i class="fas fa-university mr-2 text-blue-600"></i>Ngân hàng: <span id="bank-name-display" class="font-bold"></span></p>
                <p class="text-gray-700 text-lg font-semibold"><i class="fas fa-credit-card mr-2 text-green-600"></i>Số tài khoản: <span id="account-number-display" class="font-bold"></span></p>
                <p class="text-gray-700 text-lg font-semibold mb-4"><i class="fas fa-user-circle mr-2 text-purple-600"></i>Chủ tài khoản: <span id="account-holder-display" class="font-bold"></span></p>
                <p class="text-gray-700 text-xl font-bold">Tổng tiền đơn hàng: <span id="payment-modal-order-total" class="text-blue-700">0 VND</span></p>
            </div>

            <div class="mb-4">
                <label for="payment-amount-input" class="block text-gray-700 text-lg font-bold mb-2">Nhập số tiền thanh toán hoặc %:</label>
                <input type="text" id="payment-amount-input" class="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Ví dụ: % hoặc vnđ">
            </div>

            <div class="mb-6 bg-gray-100 p-4 rounded-lg">
                <p class="text-gray-700 text-lg font-semibold">Đã thanh toán trước: <span id="amount-paid-display" class="font-bold text-green-600">0 VND</span></p>
                <p class="text-gray-700 text-lg font-semibold">Thanh toán khi nhận hàng: <span id="remaining-payment-display" class="font-bold text-red-600">0 VND</span></p>
            </div>

            <button id="confirm-payment-btn" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg w-full transition-all duration-200 shadow-lg"><i class="fas fa-check-circle mr-2"></i>Xác nhận Thanh toán</button>
        </div>
    </div>
    
    <script>
        let shopData = JSON.parse(localStorage.getItem('shopData')) || {};

        shopData.name = shopData.name || 'Thegioididong.com';
        shopData.address = shopData.address || 'Chưa cập nhật';
        shopData.backgroundImg = shopData.backgroundImg || '';
        
        if (!shopData.advertisement || typeof shopData.advertisement !== 'object') {
            shopData.advertisement = {
                text: 'Chào mừng bạn đến với Thegioididong.com!',
                animation: 'marquee'
            };
        }
        shopData.advertisement.text = shopData.advertisement.text || 'Chào mừng bạn đến với Thegioididong.com!';
        shopData.advertisement.animation = shopData.advertisement.animation || 'marquee';

        if (!shopData.bankDetails || typeof shopData.bankDetails !== 'object') {
            shopData.bankDetails = {
                bankName: '',
                accountNumber: '',
                accountHolder: '',
                qrCodeImage: ''
            };
        }
        shopData.bankDetails.bankName = shopData.bankDetails.bankName || '';
        shopData.bankDetails.accountNumber = shopData.bankDetails.accountNumber || '';
        shopData.bankDetails.accountHolder = shopData.bankDetails.accountHolder || '';
        shopData.bankDetails.qrCodeImage = shopData.bankDetails.qrCodeImage || '';

        if (!Array.isArray(shopData.products) || shopData.products.length === 0) {
            shopData.products = [
                {
                    id: 'p1',
                    name: 'iPhone 15 Pro Max',
                    basePrice: 28000000,
                    image: 'https://placehold.co/400x400/FF0000/FFFFFF?text=iPhone+15',
                    description: 'iPhone 15 Pro Max là điện thoại thông minh cao cấp với chip A17 Bionic, camera Pro mạnh mẽ và màn hình Super Retina XDR.',
                    colors: [
                        { name: 'Natural Titanium', display_image: 'https://placehold.co/400x400/969696/FFFFFF?text=Titanium+Natural', priceImpact: 0 },
                        { name: 'Blue Titanium', display_image: 'https://placehold.co/400x400/2A52BE/FFFFFF?text=Titanium+Blue', priceImpact: 500000 },
                        { name: 'White Titanium', display_image: 'https://placehold.co/400x400/E0E0E0/000000?text=Titanium+White', priceImpact: 200000 }
                    ],
                    storages: [
                        { name: '128GB', priceImpact: 0 },
                        { name: '256GB', priceImpact: 1000000 },
                        { name: '512GB', priceImpact: 2500000 },
                        { name: '1TB', priceImpact: 4000000 }
                    ],
                    variants: [
                        { color: 'Natural Titanium', storage: '128GB', priceImpact: 0, quantity: 5, sold: 10 },
                        { color: 'Natural Titanium', storage: '256GB', priceImpact: 0, quantity: 15, sold: 5 },
                        { color: 'Natural Titanium', storage: '512GB', priceImpact: 0, quantity: 8, sold: 2 },
                        { color: 'Natural Titanium', storage: '1TB', priceImpact: 0, quantity: 4, sold: 1 },

                        { color: 'Blue Titanium', storage: '128GB', priceImpact: 0, quantity: 2, sold: 8 },
                        { color: 'Blue Titanium', storage: '256GB', priceImpact: 0, quantity: 7, sold: 3 },
                        { color: 'Blue Titanium', storage: '512GB', priceImpact: 0, quantity: 0, sold: 0 },
                        { color: 'Blue Titanium', storage: '1TB', priceImpact: 0, quantity: 9, sold: 1 },

                        { color: 'White Titanium', storage: '128GB', priceImpact: 0, quantity: 3, sold: 7 },
                        { color: 'White Titanium', storage: '256GB', priceImpact: 0, quantity: 6, sold: 4 },
                        { color: 'White Titanium', storage: '512GB', priceImpact: 0, quantity: 8, sold: 2 },
                        { color: 'White Titanium', storage: '1TB', priceImpact: 0, quantity: 9, sold: 1 },
                    ],
                    reviewsCount: 120
                },
                {
                    id: 'p2',
                    name: 'Samsung Galaxy S24 Ultra',
                    basePrice: 26000000,
                    image: 'https://placehold.co/400x400/0000FF/FFFFFF?text=S24+Ultra',
                    description: 'Samsung Galaxy S24 Ultra với S Pen tích hợp, camera 200MP và hiệu năng mạnh mẽ.',
                    colors: [
                        { name: 'Phantom Black', display_image: 'https://placehold.co/400x400/333333/FFFFFF?text=Phantom+Black', priceImpact: 0 },
                        { name: 'Gray', display_image: 'https://placehold.co/400x400/808080/FFFFFF?text=Gray', priceImpact: 300000 }
                    ],
                    storages: [
                        { name: '256GB', priceImpact: 0 },
                        { name: '512GB', priceImpact: 1200000 },
                        { name: '1TB', priceImpact: 2800000 }
                    ],
                    variants: [
                        { color: 'Phantom Black', storage: '256GB', priceImpact: 0, quantity: 3, sold: 12 },
                        { color: 'Phantom Black', storage: '512GB', priceImpact: 0, quantity: 7, sold: 8 },
                        { color: 'Phantom Black', storage: '1TB', priceImpact: 0, quantity: 10, sold: 5 },

                        { color: 'Gray', storage: '256GB', priceImpact: 0, quantity: 8, sold: 7 },
                        { color: 'Gray', storage: '512GB', priceImpact: 0, quantity: 11, sold: 4 },
                        { color: 'Gray', storage: '1TB', priceImpact: 0, quantity: 0, sold: 0 },
                    ],
                    reviewsCount: 85
                },
                {
                    id: 'p3',
                    name: 'MacBook Air M3',
                    basePrice: 23000000,
                    image: 'https://placehold.co/400x400/008000/FFFFFF?text=MacBook+Air',
                    description: 'MacBook Air M3 siêu mỏng, nhẹ và mạnh mẽ với chip M3.',
                    colors: [
                        { name: 'Silver', display_image: 'https://placehold.co/400x400/C0C0C0/000000?text=Silver', priceImpact: 0 },
                        { name: 'Space Gray', display_image: 'https://placehold.co/400x400/696969/FFFFFF?text=Space+Gray', priceImpact: 100000 }
                    ],
                    storages: [
                        { name: '256GB', priceImpact: 0 },
                        { name: '512GB', priceImpact: 1500000 },
                        { name: '1TB', priceImpact: 3000000 }
                    ],
                    variants: [
                        { color: 'Silver', storage: '256GB', priceImpact: 0, quantity: 10, sold: 5 },
                        { color: 'Silver', storage: '512GB', priceImpact: 0, quantity: 7, sold: 3 },
                        { color: 'Silver', storage: '1TB', priceImpact: 0, quantity: 4, sold: 1 },

                        { color: 'Space Gray', storage: '256GB', priceImpact: 0, quantity: 6, sold: 4 },
                        { color: 'Space Gray', storage: '512GB', priceImpact: 0, quantity: 8, sold: 2 },
                        { color: 'Space Gray', storage: '1TB', priceImpact: 0, quantity: 0, sold: 0 },
                    ],
                    reviewsCount: 50
                },
                {
                    id: 'p4',
                    name: 'Tai nghe Sony WH-1000XM5',
                    basePrice: 7000000,
                    image: 'https://placehold.co/400x400/800080/FFFFFF?text=Sony+XM5',
                    description: 'Tai nghe chống ồn hàng đầu với chất lượng âm thanh xuất sắc.',
                    colors: [
                        { name: 'Black', display_image: 'https://placehold.co/400x400/000000/FFFFFF?text=Black', priceImpact: 0 },
                        { name: 'Silver', display_image: 'https://placehold.co/400x400/C0C0C0/000000?text=Silver', priceImpact: 0 }
                    ],
                    variants: [
                        { color: 'Black', storage: null, priceImpact: 0, quantity: 10, sold: 20 },
                        { color: 'Silver', storage: null, priceImpact: 0, quantity: 5, sold: 15 }
                    ],
                    reviewsCount: 200
                }
            ];
        }

        if (!shopData.vouchers || typeof shopData.vouchers !== 'object') {
            shopData.vouchers = {
                'MPVT': 'freeship',
                'SALE10': 0.10,
                'GIAM50K': 50000
            };
        }

        if (!Array.isArray(shopData.orders)) {
            shopData.orders = [];
        }

        if (!Array.isArray(shopData.cart)) {
            shopData.cart = [];
        }


        function saveData() {
            localStorage.setItem('shopData', JSON.stringify(shopData));
        }

        function generateId() {
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
                var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
        }

        function formatCurrency(amount) {
            return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
        }

        const loadingOverlay = document.createElement('div');
        loadingOverlay.id = 'loadingIndicator';
        loadingOverlay.className = 'loading-overlay hidden';
        loadingOverlay.innerHTML = '<div class="loading-spinner"></div>';
        document.body.appendChild(loadingOverlay);

        const messageDisplay = document.createElement('div');
        messageDisplay.id = 'messageDisplay';
        messageDisplay.className = 'message hidden fixed top-24 right-6 p-4 rounded-lg shadow-lg z-50';
        document.body.appendChild(messageDisplay);

        function showLoading() { loadingOverlay.classList.remove('hidden'); }
        function hideLoading() { loadingOverlay.classList.add('hidden'); }
        function showMessage(message, type = 'info') {
            messageDisplay.textContent = message;
            messageDisplay.className = `message ${type} fixed top-24 right-6 p-4 rounded-lg shadow-lg z-50`;
            messageDisplay.classList.remove('hidden');
            setTimeout(() => {
                messageDisplay.classList.add('hidden');
                messageDisplay.textContent = '';
                messageDisplay.className = 'message hidden fixed top-24 right-6 p-4 rounded-lg shadow-lg z-50';
            }, 3000);
        }

        const shopNameDisplay = document.getElementById('shop-name-display');
        const shopAddressDisplay = document.getElementById('shop-address-display');
        const pageTitle = document.getElementById('page-title');
        const bodyElement = document.body;
        const advertisementBannerContainer = document.getElementById('advertisement-banner-container');
        const advertisementContent = document.getElementById('advertisement-content');
        const headerProductSearchInput = document.getElementById('header-product-search-input');

        const productDetailModal = document.getElementById('product-detail-modal');
        const orderCreationModal = document.getElementById('order-creation-modal');
        const editShippingOrderModal = document.getElementById('edit-shipping-order-modal');
        const shopManagementModal = document.getElementById('shop-management-modal');
        const shopSettingsModal = document.getElementById('shop-settings-modal');
        const shopAnalyticsModal = document.getElementById('shop-analytics-modal');
        const cartModal = document.getElementById('cart-modal');
        const paymentVoucherModal = document.getElementById('payment-voucher-modal');

        const closeProductModalBtn = document.getElementById('close-product-modal');
        const closeOrderModalBtn = document.getElementById('close-order-modal');
        const closeEditShippingModalBtn = document.getElementById('close-edit-shipping-modal');
        const closeManagementModalBtn = document.getElementById('close-management-modal');
        const closeSettingsModalBtn = document.getElementById('close-settings-modal');
        const closeAnalyticsModalBtn = document.getElementById('close-analytics-modal');
        const closeCartModalBtn = document.getElementById('close-cart-modal');
        const closePaymentVoucherModalBtn = document.getElementById('close-payment-voucher-modal');

        const showProductsBtn = document.getElementById('show-products-btn');
        const showCreatedOrdersBtn = document.getElementById('show-created-orders-btn');
        const showShippingOrdersBtn = document.getElementById('show-shipping-orders-btn');
        const showDeliveredOrdersBtn = document.getElementById('show-delivered-orders-btn');
        const openManagementModalBtn = document.getElementById('open-management-modal-btn');
        const openSettingsModalBtn = document.getElementById('open-settings-modal-btn');
        const openShopAnalyticsModalBtn = document.getElementById('open-shop-analytics-modal-btn');
        const openCartModalBtn = document.getElementById('open-cart-modal-btn');

        const productListSection = document.getElementById('product-list-section');
        const createdOrdersSection = document.getElementById('created-orders-section');
        const shippingOrdersSection = document.getElementById('shipping-orders-section');
        const deliveredOrdersSection = document.getElementById('delivered-orders-section');

        const allTabButtons = document.querySelectorAll('.tab-button');
        const allSections = document.querySelectorAll('section > div');

        const modalProductName = document.getElementById('modal-product-name');
        const modalProductImage = document.getElementById('modal-product-image');
        const modalProductBasePrice = document.getElementById('modal-product-base-price');
        const modalProductPriceDisplay = document.getElementById('modal-product-price-display');
        const modalProductDiscountDisplay = document.getElementById('modal-product-discount-display');
        const modalProductSold = document.getElementById('modal-product-sold');
        const modalProductRemaining = document.getElementById('modal-product-remaining');
        const modalProductDescription = document.getElementById('modal-product-description');
        const modalShippingCostDisplay = document.getElementById('modal-shipping-cost-display');
        const productOptionsContainer = document.getElementById('product-options');
        const voucherCodeInput = document.getElementById('voucher-code');
        const applyVoucherBtn = document.getElementById('apply-voucher-btn');
        const buyNowDetailBtn = document.getElementById('buy-now-detail-btn');
        const addToCartDetailBtn = document.getElementById('add-to-cart-detail-btn');
        const fixedShippingCost = 0;

        const orderIdDisplay = document.getElementById('order-id-display');
        const orderProductsSummary = document.getElementById('order-products-summary');
        const customerNameInput = document.getElementById('customer-name');
        const customerPhoneInput = document.getElementById('customer-phone');
        const customerAddressInput = document.getElementById('customer-address');
        const orderLocationInput = document.getElementById('order-location');
        const estimatedDeliveryDateInput = document.getElementById('estimated-delivery-date');
        const orderForm = document.getElementById('order-form');
        const orderStatusSteps = document.querySelectorAll('.order-status-step');

        const editShippingOrderIdDisplay = document.getElementById('edit-shipping-order-id-display');
        const editShippingOrderHiddenId = document.getElementById('edit-shipping-order-hidden-id');
        const editOrderLocationInput = document.getElementById('edit-order-location');
        const editEstimatedDeliveryDateInput = document.getElementById('edit-estimated-delivery-date');
        const editShippingOrderForm = document.getElementById('edit-shipping-order-form');

        const cartCountSpan = document.getElementById('cart-count');
        const searchCartItemsInput = document.getElementById('search-cart-items');
        const clearSearchCartItemsBtn = document.getElementById('clear-search-cart-items');
        const cartItemsList = document.getElementById('cart-items-list');
        const cartTotalAmountSpan = document.getElementById('cart-total-amount');
        const buyAllCartBtn = document.getElementById('buy-all-cart-btn');

        const productManagementList = document.getElementById('product-management-list');
        const addEditProductTitle = document.getElementById('add-edit-product-title');
        const addEditProductForm = document.getElementById('add-edit-product-form');
        const editProductIdInput = document.getElementById('edit-product-id');
        const newProductNameInput = document.getElementById('new-product-name');
        const newProductBasePriceInput = document.getElementById('new-product-base-price');
        const newProductImageInput = document.getElementById('new-product-image');
        const newProductDescriptionInput = document.getElementById('new-product-description');
        const newProductReviewsInput = document.getElementById('new-product-reviews');
        const colorOptionsContainer = document.getElementById('color-options-container');
        const addColorOptionBtn = document.getElementById('add-color-option-btn');
        const storageOptionsContainer = document.getElementById('storage-options-container');
        const addStorageOptionBtn = document.getElementById('add-storage-option-btn');
        const variantsContainer = document.getElementById('variants-container');
        const addVariantBtn = document.getElementById('add-variant-btn');
        const submitProductBtn = document.getElementById('submit-product-btn');
        const cancelEditBtn = document.getElementById('cancel-edit-btn');
        const addVoucherForm = document.getElementById('add-voucher-form');
        const newVoucherCodeInput = document.getElementById('new-voucher-code');
        const newVoucherValueInput = document.getElementById('new-voucher-value');
        const currentVouchersList = document.getElementById('current-vouchers-list');

        const shopSettingsForm = document.getElementById('shop-settings-form');
        const shopNameInput = document.getElementById('shop-name-input');
        const shopAddressInput = document.getElementById('shop-address-input');
        const backgroundImageURLInput = document.getElementById('background-image-url');
        const advertisementTextInput = document.getElementById('advertisement-text-input');
        const advertisementAnimationSelect = document.getElementById('advertisement-animation-select');
        const bankNameInput = document.getElementById('bank-name-input');
        const accountNumberInput = document.getElementById('account-number-input');
        const accountHolderInput = document.getElementById('account-holder-input');
        const qrCodeImageURLInput = document.getElementById('qr-code-image-url-input');
        const uploadQrCodeBtn = document.getElementById('upload-qr-code-btn');

        const reportStartDateInput = document.getElementById('report-start-date');
        const reportEndDateInput = document.getElementById('report-end-date');
        const generateReportBtn = document.getElementById('generate-report-btn');
        const totalRevenueDisplay = document.getElementById('total-revenue-display');
        const totalOrdersDisplay = document.getElementById('total-orders-display');
        const topSellingProductsList = document.getElementById('top-selling-products-list');

        const qrCodeDisplay = document.getElementById('qr-code-display');
        const bankNameDisplay = document.getElementById('bank-name-display');
        const accountNumberDisplay = document.getElementById('account-number-display');
        const accountHolderDisplay = document.getElementById('account-holder-display');
        const paymentModalOrderTotal = document.getElementById('payment-modal-order-total');
        const paymentAmountInput = document.getElementById('payment-amount-input');
        const amountPaidDisplay = document.getElementById('amount-paid-display');
        const remainingPaymentDisplay = document.getElementById('remaining-payment-display');
        const confirmPaymentBtn = document.getElementById('confirm-payment-btn');

        let currentSelectedProduct = null;
        let currentSelectedOptions = {};
        let currentCalculatedPrice = 0;
        let currentAppliedVoucher = null;
        let productsToOrder = [];
        let currentOrderForPayment = null;
        let isBuyNowFlow = false; 

        function loadShopSettings() {
            shopNameDisplay.innerHTML = `<i class="fas fa-mobile-alt mr-2 text-gray-700"></i><span class="text-gray-900">${shopData.name.replace('.com', '')}</span><span class="text-red-600">.com</span>`;
            pageTitle.textContent = shopData.name;
            shopAddressDisplay.textContent = shopData.address;
            if (shopData.backgroundImg) {
                bodyElement.style.backgroundImage = `url('${shopData.backgroundImg}')`;
            } else {
                bodyElement.style.backgroundImage = 'none';
                bodyElement.style.backgroundColor = '#fdf8f4';
            }
            shopNameInput.value = shopData.name;
            shopAddressInput.value = shopData.address;
            backgroundImageURLInput.value = shopData.backgroundImg;
            advertisementTextInput.value = shopData.advertisement.text;
            advertisementAnimationSelect.value = shopData.advertisement.animation;
            bankNameInput.value = shopData.bankDetails.bankName || '';
            accountNumberInput.value = shopData.bankDetails.accountNumber || '';
            accountHolderInput.value = shopData.bankDetails.accountHolder || '';
            qrCodeImageURLInput.value = shopData.bankDetails.qrCodeImage || '';
            updateAdvertisementBanner();
        }

        function updateAdvertisementBanner() {
            const adText = shopData.advertisement.text;
            const adAnimation = shopData.advertisement.animation;

            if (adText) {
                advertisementContent.textContent = adText;
                advertisementBannerContainer.classList.remove('hidden');
                advertisementBannerContainer.classList.remove('marquee', 'flicker');

                if (adAnimation === 'marquee') {
                    advertisementBannerContainer.classList.add('marquee');
                } else if (adAnimation === 'flicker') {
                    advertisementBannerContainer.classList.add('flicker');
                }
            } else {
                advertisementBannerContainer.classList.add('hidden');
                advertisementContent.textContent = '';
            }
        }

        shopSettingsForm.addEventListener('submit', (e) => {
            e.preventDefault();
            showLoading();
            if (!shopData.bankDetails) {
                shopData.bankDetails = {};
            }
            if (!shopData.advertisement) {
                shopData.advertisement = {};
            }

            shopData.name = shopNameInput.value.trim();
            shopData.address = shopAddressInput.value.trim();
            shopData.backgroundImg = backgroundImageURLInput.value.trim();
            shopData.advertisement.text = advertisementTextInput.value.trim();
            shopData.advertisement.animation = advertisementAnimationSelect.value;
            shopData.bankDetails.bankName = bankNameInput.value.trim();
            shopData.bankDetails.accountNumber = accountNumberInput.value.trim();
            shopData.bankDetails.accountHolder = accountHolderInput.value.trim();
            shopData.bankDetails.qrCodeImage = qrCodeImageURLInput.value.trim();
            saveData();
            loadShopSettings();
            hideLoading();
            showMessage('Cài đặt cửa hàng đã được lưu!', 'success');
            closeModal(shopSettingsModal);
        });

        document.getElementById('upload-main-image-btn').addEventListener('click', () => {
            const imageUrl = prompt('Nhập URL hình ảnh sản phẩm (URL trực tiếp, GitHub Raw, hoặc chuỗi Base64):');
            if (imageUrl) {
                newProductImageInput.value = imageUrl;
            }
        });

        document.getElementById('upload-background-btn').addEventListener('click', () => {
            const imageUrl = prompt('Nhập URL hình ảnh nền (URL trực tiếp, GitHub Raw, hoặc chuỗi Base64):');
            if (imageUrl) {
                backgroundImageURLInput.value = imageUrl;
            }
        });

        uploadQrCodeBtn.addEventListener('click', () => {
            const imageUrl = prompt('Nhập URL hình ảnh QR Code ngân hàng (URL trực tiếp, GitHub Raw, hoặc chuỗi Base64):');
            if (imageUrl) {
                qrCodeImageURLInput.value = imageUrl;
            }
        });

        function openModal(modalElement) {
            modalElement.classList.remove('hidden');
            modalElement.classList.add('active');
            document.body.classList.add('overflow-hidden');
        }

        function closeModal(modalElement) {
            modalElement.classList.remove('active');
            modalElement.addEventListener('transitionend', () => {
                modalElement.classList.add('hidden');
                document.body.classList.remove('overflow-hidden');
            }, { once: true });
        }

        function showSection(sectionId, clickedButton) {
            allSections.forEach(section => section.classList.add('hidden'));
            document.getElementById(sectionId).classList.remove('hidden');

            allTabButtons.forEach(btn => btn.classList.remove('active'));
            if (clickedButton) {
                clickedButton.classList.add('active');
            }
        }

        closeProductModalBtn.addEventListener('click', () => closeModal(productDetailModal));
        closeOrderModalBtn.addEventListener('click', () => closeModal(orderCreationModal));
        closeEditShippingModalBtn.addEventListener('click', () => closeModal(editShippingOrderModal));
        closeManagementModalBtn.addEventListener('click', () => closeModal(shopManagementModal));
        closeSettingsModalBtn.addEventListener('click', () => closeModal(shopSettingsModal));
        closeAnalyticsModalBtn.addEventListener('click', () => closeModal(shopAnalyticsModal));
        closeCartModalBtn.addEventListener('click', () => closeModal(cartModal));
        closePaymentVoucherModalBtn.addEventListener('click', () => closeModal(paymentVoucherModal));


        productDetailModal.addEventListener('click', (e) => { if (e.target === productDetailModal) closeModal(productDetailModal); });
        orderCreationModal.addEventListener('click', (e) => { if (e.target === orderCreationModal) closeModal(orderCreationModal); });
        editShippingOrderModal.addEventListener('click', (e) => { if (e.target === editShippingOrderModal) closeModal(editShippingOrderModal); });
        shopManagementModal.addEventListener('click', (e) => { if (e.target === shopManagementModal) closeModal(shopManagementModal); });
        shopSettingsModal.addEventListener('click', (e) => { if (e.target === shopSettingsModal) closeModal(shopSettingsModal); });
        shopAnalyticsModal.addEventListener('click', (e) => { if (e.target === shopAnalyticsModal) closeModal(shopAnalyticsModal); });
        cartModal.addEventListener('click', (e) => { if (e.target === cartModal) closeModal(cartModal); });
        paymentVoucherModal.addEventListener('click', (e) => { if (e.target === paymentVoucherModal) closeModal(paymentVoucherModal); });


        openManagementModalBtn.addEventListener('click', () => {
            openModal(shopManagementModal);
            renderProductManagementList();
            renderVouchersList();
            resetAddEditProductForm();
        });
        openSettingsModalBtn.addEventListener('click', () => openModal(shopSettingsModal));
        openShopAnalyticsModalBtn.addEventListener('click', () => openModal(shopAnalyticsModal));
        openCartModalBtn.addEventListener('click', () => {
            openModal(cartModal);
            renderCart();
        });

        showProductsBtn.addEventListener('click', (e) => showSection('product-list-section', e.target));
        showCreatedOrdersBtn.addEventListener('click', (e) => showSection('created-orders-section', e.target));
        showShippingOrdersBtn.addEventListener('click', (e) => showSection('shipping-orders-section', e.target));
        showDeliveredOrdersBtn.addEventListener('click', (e) => showSection('delivered-orders-section', e.target));

        document.querySelectorAll('.toggle-visibility-btn').forEach(button => {
            button.addEventListener('click', function() {
                const targetId = this.dataset.targetId;
                const targetInput = document.getElementById(targetId);
                if (targetInput) {
                    if (targetInput.classList.contains('blurred-content')) {
                        targetInput.classList.remove('blurred-content');
                        this.innerHTML = '<i class="fas fa-eye-slash"></i>';
                    } else {
                        targetInput.classList.add('blurred-content');
                        this.innerHTML = '<i class="fas fa-eye"></i>';
                    }
                }
            });
        });

        headerProductSearchInput.addEventListener('input', () => {
            renderProducts(headerProductSearchInput.value.trim());
        });

        function renderProductCard(product) {
            const card = document.createElement('div');
            card.className = 'product-card bg-white rounded-xl shadow-lg p-6 flex flex-col items-center text-center hover:shadow-xl transition-shadow duration-300 relative';
            
            const starsHtml = '<div class="star-rating text-yellow-400 mb-2">' +
                               '<i class="fas fa-star"></i><i class="fas fa-star"></i><i class="fas fa-star"></i><i class="fas fa-star"></i><i class="fas fa-star"></i>' +
                               ` <span class="text-gray-600 text-sm">(${product.reviewsCount || 0} đánh giá)</span>` +
                               '</div>';

            card.innerHTML = `
                <img src="${product.image}" onerror="this.onerror=null;this.src='https://placehold.co/300x200/cccccc/333333?text=No+Image';" alt="${product.name}" class="w-full h-48 object-cover rounded-lg mb-4 shadow-md">
                <h3 class="text-xl font-semibold mb-2 text-gray-900">${product.name}</h3>
                <p class="text-lg text-gray-700">Giá cơ bản: <span class="font-bold">${formatCurrency(product.basePrice)}</span></p>
                ${starsHtml}
                <button data-product-id="${product.id}" class="view-product-btn mt-4 bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-5 rounded-full transition-all duration-200 shadow-md">Xem chi tiết</button>
            `;
            return card;
        }

        function renderProducts(searchTerm = '') {
            const productGrid = document.getElementById('product-grid');
            productGrid.innerHTML = '';
            
            const lowerCaseSearchTerm = searchTerm.toLowerCase();
            const filteredProducts = shopData.products.filter(product => 
                product.name.toLowerCase().includes(lowerCaseSearchTerm) ||
                product.description.toLowerCase().includes(lowerCaseSearchTerm)
            );

            if (filteredProducts.length === 0) {
                productGrid.innerHTML = '<p class="text-gray-500 italic text-center col-span-full">Không tìm thấy sản phẩm nào phù hợp.</p>';
                return;
            }
            filteredProducts.forEach(product => {
                productGrid.appendChild(renderProductCard(product));
            });

            document.querySelectorAll('.view-product-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const productId = e.target.dataset.productId;
                    const product = shopData.products.find(p => p.id === productId);
                    if (product) {
                        displayProductDetail(product);
                    }
                });
            });
        }

        function displayProductDetail(product) {
            currentSelectedProduct = product;
            currentSelectedOptions = {};
            currentAppliedVoucher = null;
            voucherCodeInput.value = '';

            modalProductName.textContent = product.name;
            modalProductImage.src = product.image;
            modalProductBasePrice.textContent = formatCurrency(product.basePrice);
            modalProductDescription.textContent = product.description;
            modalProductSold.textContent = product.sold || 0;
            
            let totalRemaining = 0;
            if (product.variants && product.variants.length > 0) {
                totalRemaining = product.variants.reduce((sum, variant) => sum + (variant.quantity || 0), 0);
            } else {
                totalRemaining = product.quantity || 0;
            }
            modalProductRemaining.textContent = totalRemaining;

            productOptionsContainer.innerHTML = '';
            
            if (product.colors && product.colors.length > 0) {
                const colorDiv = document.createElement('div');
                colorDiv.className = 'flex flex-col space-y-2';
                colorDiv.innerHTML = '<p class="text-gray-700 font-semibold mb-2">Màu sắc:</p>';
                const colorButtonsDiv = document.createElement('div');
                colorButtonsDiv.className = 'flex flex-wrap gap-2';
                product.colors.forEach(color => {
                    const btn = document.createElement('button');
                    btn.className = 'option-button bg-gray-200 hover:bg-gray-300 text-gray-800 py-2 px-4 rounded-full transition-all duration-200 text-sm';
                    btn.textContent = color.name;
                    btn.dataset.type = 'color';
                    btn.dataset.value = color.name;
                    btn.dataset.priceImpact = color.priceImpact;
                    btn.dataset.displayImage = color.display_image || '';
                    colorButtonsDiv.appendChild(btn);
                    btn.addEventListener('click', () => selectOption('color', color.name, color.priceImpact, btn, color.display_image));
                });
                colorDiv.appendChild(colorButtonsDiv);
                productOptionsContainer.appendChild(colorDiv);
                selectOption('color', product.colors[0].name, product.colors[0].priceImpact, colorButtonsDiv.children[0], product.colors[0].display_image);
            } else {
                currentSelectedOptions.color = null;
            }

            if (product.storages && product.storages.length > 0) {
                const storageDiv = document.createElement('div');
                storageDiv.className = 'flex flex-col space-y-2';
                storageDiv.innerHTML = '<p class="text-gray-700 font-semibold mb-2">Dung lượng:</p>';
                const storageButtonsDiv = document.createElement('div');
                storageButtonsDiv.className = 'flex flex-wrap gap-2';
                product.storages.forEach(storage => {
                    const btn = document.createElement('button');
                    btn.className = 'option-button bg-gray-200 hover:bg-gray-300 text-gray-800 py-2 px-4 rounded-full transition-all duration-200 text-sm';
                    btn.textContent = storage.name;
                    btn.dataset.type = 'storage';
                    btn.dataset.value = storage.name;
                    btn.dataset.priceImpact = storage.priceImpact;
                    storageButtonsDiv.appendChild(btn);
                    btn.addEventListener('click', () => selectOption('storage', storage.name, storage.priceImpact, btn));
                });
                storageDiv.appendChild(storageButtonsDiv);
                productOptionsContainer.appendChild(storageDiv);
                selectOption('storage', product.storages[0].name, product.storages[0].priceImpact, storageButtonsDiv.children[0]);
            } else {
                currentSelectedOptions.storage = null;
            }

            calculateProductPrice();

            openModal(productDetailModal);
        }

        function selectOption(type, value, priceImpact, button, displayImage = null) {
            document.querySelectorAll(`.option-button[data-type="${type}"]`).forEach(btn => {
                btn.classList.remove('selected');
            });
            if (button) {
                button.classList.add('selected');
            }

            currentSelectedOptions[type] = { value, priceImpact: parseFloat(priceImpact) };
            if (type === 'color' && displayImage) {
                modalProductImage.src = displayImage;
            } else if (type === 'color' && !displayImage) {
                modalProductImage.src = currentSelectedProduct.image;
            }
            calculateProductPrice();
        }

        function calculateProductPrice() {
            let basePrice = currentSelectedProduct.basePrice;
            let currentPrice = basePrice;
            let discountAmount = 0;

            for (const type in currentSelectedOptions) {
                currentPrice += (currentSelectedOptions[type] && currentSelectedOptions[type].priceImpact) || 0;
            }

            const selectedColor = currentSelectedOptions.color ? currentSelectedOptions.color.value : null;
            const selectedStorage = currentSelectedOptions.storage ? currentSelectedOptions.storage.value : null;

            if (currentSelectedProduct.variants && currentSelectedProduct.variants.length > 0) {
                const matchingVariant = currentSelectedProduct.variants.find(variant => {
                    const colorMatches = (selectedColor === null && variant.color === null) || (selectedColor !== null && variant.color === selectedColor);
                    const storageMatches = (selectedStorage === null && variant.storage === null) || (selectedStorage !== null && variant.storage === selectedStorage);
                    return colorMatches && storageMatches;
                });
                
                if (matchingVariant) {
                    currentPrice += matchingVariant.priceImpact || 0;
                    modalProductRemaining.textContent = matchingVariant.quantity || 0;
                    if (matchingVariant.quantity === 0) {
                        buyNowDetailBtn.disabled = true;
                        buyNowDetailBtn.textContent = 'Hết hàng';
                        buyNowDetailBtn.classList.add('bg-gray-400', 'hover:bg-gray-400');
                        buyNowDetailBtn.classList.remove('bg-yellow-400', 'hover:bg-yellow-500');
                        
                        addToCartDetailBtn.disabled = true;
                        addToCartDetailBtn.classList.add('bg-gray-400', 'hover:bg-gray-400');
                        addToCartDetailBtn.classList.remove('bg-yellow-400', 'hover:bg-yellow-500');
                        showMessage('Sản phẩm này hiện đang hết hàng!', 'error');
                    } else {
                        buyNowDetailBtn.disabled = false;
                        buyNowDetailBtn.textContent = 'Mua Ngay';
                        buyNowDetailBtn.classList.remove('bg-gray-400', 'hover:bg-gray-400');
                        buyNowDetailBtn.classList.add('bg-yellow-400', 'hover:bg-yellow-500');

                        addToCartDetailBtn.disabled = false;
                        addToCartDetailBtn.classList.remove('bg-gray-400', 'hover:bg-gray-400');
                        addToCartDetailBtn.classList.add('bg-yellow-400', 'hover:bg-yellow-500');
                    }
                } else {
                    modalProductRemaining.textContent = '0';
                    buyNowDetailBtn.disabled = true;
                    buyNowDetailBtn.textContent = 'Không có biến thể này';
                    buyNowDetailBtn.classList.add('bg-gray-400', 'hover:bg-gray-400');
                    buyNowDetailBtn.classList.remove('bg-yellow-400', 'hover:bg-yellow-500');

                    addToCartDetailBtn.disabled = true;
                    addToCartDetailBtn.classList.add('bg-gray-400', 'hover:bg-gray-400');
                    addToCartDetailBtn.classList.remove('bg-yellow-400', 'hover:bg-yellow-500');
                    showMessage('Đang tải!', 'error');
                }
            } else {
                modalProductRemaining.textContent = currentSelectedProduct.quantity || 0;
                if (currentSelectedProduct.quantity === 0 || !currentSelectedProduct.quantity) {
                        buyNowDetailBtn.disabled = true;
                        buyNowDetailBtn.textContent = 'Hết hàng';
                        buyNowDetailBtn.classList.add('bg-gray-400', 'hover:bg-gray-400');
                        buyNowDetailBtn.classList.remove('bg-yellow-400', 'hover:bg-yellow-500');
                        
                        addToCartDetailBtn.disabled = true;
                        addToCartDetailBtn.classList.add('bg-gray-400', 'hover:bg-gray-400');
                        addToCartDetailBtn.classList.remove('bg-yellow-400', 'hover:bg-yellow-500');
                        showMessage('Sản phẩm này hiện đang hết hàng!', 'error');
                    } else {
                        buyNowDetailBtn.disabled = false;
                        buyNowDetailBtn.textContent = 'Mua Ngay';
                        buyNowDetailBtn.classList.remove('bg-gray-400', 'hover:bg-gray-400');
                        buyNowDetailBtn.classList.add('bg-yellow-400', 'hover:bg-yellow-500');

                        addToCartDetailBtn.disabled = false;
                        addToCartDetailBtn.classList.remove('bg-gray-400', 'hover:bg-gray-400');
                        addToCartDetailBtn.classList.add('bg-yellow-400', 'hover:bg-yellow-500');
                    }
            }

            if (currentAppliedVoucher) {
                if (typeof currentAppliedVoucher.value === 'number') {
                    if (currentAppliedVoucher.value < 1) {
                        discountAmount = currentPrice * currentAppliedVoucher.value;
                        currentPrice -= discountAmount;
                    } else {
                        discountAmount = currentAppliedVoucher.value;
                        currentPrice -= discountAmount;
                        if (currentPrice < 0) currentPrice = 0;
                    }
                    modalProductPriceDisplay.classList.add('price-original');
                    modalProductDiscountDisplay.classList.remove('hidden');
                    modalProductDiscountDisplay.textContent = `Giá sau giảm: ${formatCurrency(currentPrice)}`;
                    let priceBeforeVoucher = basePrice + Object.values(currentSelectedOptions).reduce((sum, opt) => sum + (opt ? opt.priceImpact : 0), 0);
                    if (currentSelectedProduct.variants && currentSelectedProduct.variants.length > 0) {
                        const variantPriceImpact = currentSelectedProduct.variants.find(variant => {
                            const colorMatches = (selectedColor === null && variant.color === null) || (selectedColor !== null && variant.color === selectedColor);
                            const storageMatches = (selectedStorage === null && variant.storage === null) || (selectedStorage !== null && variant.storage === selectedStorage);
                            return colorMatches && storageMatches;
                        })?.priceImpact || 0;
                        priceBeforeVoucher += variantPriceImpact;
                    }
                    modalProductPriceDisplay.textContent = formatCurrency(priceBeforeVoucher);

                } else if (currentAppliedVoucher.value === 'freeship') {
                    modalProductPriceDisplay.classList.remove('price-original');
                    modalProductDiscountDisplay.classList.add('hidden');
                    modalProductPriceDisplay.textContent = formatCurrency(currentPrice);
                }

            } else {
                modalProductPriceDisplay.classList.remove('price-original');
                modalProductDiscountDisplay.classList.add('hidden');
                modalProductPriceDisplay.textContent = formatCurrency(currentPrice);
            }
            
            currentCalculatedPrice = currentPrice;
            updateShippingCostDisplay();
        }

        applyVoucherBtn.addEventListener('click', () => {
            const voucherCode = voucherCodeInput.value.trim().toUpperCase();
            const voucherValue = shopData.vouchers[voucherCode];

            if (voucherValue !== undefined) {
                currentAppliedVoucher = { code: voucherCode, value: voucherValue };
                calculateProductPrice();
                showMessage(`Áp dụng voucher "${voucherCode}" thành công!`, 'success');
            } else {
                currentAppliedVoucher = null;
                calculateProductPrice();
                showMessage('Mã voucher không hợp lệ hoặc không tồn tại.', 'error');
            }
        });

        function updateShippingCostDisplay() {
            if (currentAppliedVoucher && currentAppliedVoucher.value === 'freeship') {
                modalShippingCostDisplay.innerHTML = `<i class="fas fa-shipping-fast mr-2 text-green-500"></i> Phí vận chuyển: <span class="font-semibold text-green-600">Miễn phí vận chuyển <i class="fas fa-tag ml-1 text-orange-400"></i></span>`;
            } else {
                modalShippingCostDisplay.innerHTML = `<i class="fas fa-shipping-fast mr-2 text-green-500"></i> Phí vận chuyển: <span class="font-semibold">${formatCurrency(fixedShippingCost)}</span>`;
            }
        }

        addVoucherForm.addEventListener('submit', (event) => {
            event.preventDefault();
            showLoading();

            const code = newVoucherCodeInput.value.trim().toUpperCase();
            let valueInput = newVoucherValueInput.value.trim();
            let value;

            if (!code) {
                showMessage('Vui lòng điền mã voucher!', 'error');
                hideLoading();
                return;
            }

            if (valueInput.toLowerCase() === 'freeship' || valueInput.toLowerCase() === 'miễn phí vận chuyển') {
                value = 'freeship';
            } else {
                value = parseFloat(valueInput);
                if (isNaN(value) || value <= 0) {
                    showMessage('Giá trị voucher không hợp lệ. Vui lòng nhập số hoặc "freeship".', 'error');
                    hideLoading();
                    return;
                }
            }
            
            if (shopData.vouchers.hasOwnProperty(code)) {
                showMessage('Mã voucher này đã tồn tại!', 'error');
                hideLoading();
                return;
            }

            shopData.vouchers[code] = value;
            saveData();
            renderVouchersList();
            addVoucherForm.reset();
            hideLoading();
            showMessage('Thêm voucher thành công!', 'success');
        });

        function renderVouchersList() {
            currentVouchersList.innerHTML = '';
            const voucherKeys = Object.keys(shopData.vouchers);
            if (voucherKeys.length === 0) {
                currentVouchersList.innerHTML = '<p class="text-gray-500 italic">Chưa có voucher nào.</p>';
                return;
            }
            voucherKeys.forEach(code => {
                const value = shopData.vouchers[code];
                const displayValue = typeof value === 'number' ? (value < 1 ? `${(value * 100).toFixed(0)}%` : formatCurrency(value)) : 'Miễn phí vận chuyển';
                const voucherItem = document.createElement('div');
                voucherItem.className = 'flex justify-between items-center bg-gray-100 p-3 rounded-lg shadow-sm';
                voucherItem.innerHTML = `
                    <span class="font-semibold text-gray-800">${code}</span>
                    <span class="text-blue-600">${displayValue}</span>
                    <button data-voucher-code="${code}" class="delete-voucher-btn text-red-500 hover:text-red-700 ml-4"><i class="fas fa-trash-alt"></i></button>
                `;
                currentVouchersList.appendChild(voucherItem);
            });

            document.querySelectorAll('.delete-voucher-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const codeToDelete = e.currentTarget.dataset.voucherCode;
                    showConfirmModal(`Bạn có chắc chắn muốn xóa voucher "${codeToDelete}"?`, () => {
                        delete shopData.vouchers[codeToDelete];
                        saveData();
                        renderVouchersList();
                        showMessage('Voucher đã được xóa.', 'success');
                    });
                });
            });
        }

        function renderOptionInput(containerId, type, name = '', priceImpact = 0, displayImage = '') {
            const container = document.getElementById(containerId);
            const div = document.createElement('div');
            div.className = 'flex items-center gap-2 bg-white p-3 rounded-md shadow-sm w-full';
            div.innerHTML = `
                <input type="text" class="option-name-input shadow appearance-none border rounded w-1/3 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="${type === 'color' ? 'Tên màu (ví dụ: Đỏ)' : 'Tên dung lượng (ví dụ: 128GB)'}" value="${name}">
                <input type="number" step="any" class="option-price-impact-input shadow appearance-none border rounded w-1/3 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Giá tác động (VND)" value="${priceImpact}">
                ${type === 'color' ? `<input type="url" class="option-display-image-input shadow appearance-none border rounded w-1/3 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="URL hình ảnh hiển thị (URL trực tiếp, GitHub Raw, hoặc Base64)" value="${displayImage}">` : ''}
                <button type="button" class="remove-option-btn text-red-500 hover:text-red-700 p-2 rounded-full flex-shrink-0"><i class="fas fa-times-circle"></i></button>
            `;
            container.appendChild(div);

            div.querySelector('.remove-option-btn').addEventListener('click', (e) => {
                div.remove();
                renderVariantsSection();
            });
        }

        function renderVariantInput(color = '', storage = '', quantity = 0, priceImpact = 0) {
            const container = document.getElementById('variants-container');
            const div = document.createElement('div');
            div.className = 'flex flex-wrap items-center gap-2 bg-white p-3 rounded-md shadow-sm';
            const colors = Array.from(colorOptionsContainer.querySelectorAll('.option-name-input')).map(input => input.value);
            const storages = Array.from(storageOptionsContainer.querySelectorAll('.option-name-input')).map(input => input.value);

            div.innerHTML = `
                <select class="variant-color-select shadow appearance-none border rounded w-full md:w-1/4 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <option value="">Chọn màu</option>
                    ${colors.map(c => `<option value="${c}" ${c === color ? 'selected' : ''}>${c}</option>`).join('')}
                </select>
                <select class="variant-storage-select shadow appearance-none border rounded w-full md:w-1/4 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <option value="">Chọn dung lượng</option>
                    ${storages.map(s => `<option value="${s}" ${s === storage ? 'selected' : ''}>${s}</option>`).join('')}
                </select>
                <input type="number" class="variant-quantity-input shadow appearance-none border rounded w-full md:w-1/6 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Số lượng" value="${quantity}" min="0">
                <input type="number" step="any" class="variant-price-impact-input shadow appearance-none border rounded w-full md:w-1/5 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Giá tác động" value="${priceImpact}">
                <button type="button" class="remove-variant-btn text-red-500 hover:text-red-700 p-2 rounded-full"><i class="fas fa-times-circle"></i></button>
            `;
            container.appendChild(div);

            div.querySelector('.remove-variant-btn').addEventListener('click', (e) => div.remove());
        }

        function renderVariantsSection() {
            const existingVariants = Array.from(variantsContainer.children).map(div => ({
                color: div.querySelector('.variant-color-select')?.value || '',
                storage: div.querySelector('.variant-storage-select')?.value || '',
                quantity: parseInt(div.querySelector('.variant-quantity-input')?.value) || 0,
                priceImpact: parseFloat(div.querySelector('.variant-price-impact-input')?.value) || 0
            }));
            variantsContainer.innerHTML = '';
            existingVariants.forEach(variant => renderVariantInput(variant.color, variant.storage, variant.quantity, variant.priceImpact));
        }

        addColorOptionBtn.addEventListener('click', () => renderOptionInput('color-options-container', 'color'));
        addStorageOptionBtn.addEventListener('click', () => renderOptionInput('storage-options-container', 'storage'));
        addVariantBtn.addEventListener('click', () => renderVariantInput());

        colorOptionsContainer.addEventListener('input', (e) => {
            if (e.target.classList.contains('option-name-input')) {
                renderVariantsSection();
            }
        });
        storageOptionsContainer.addEventListener('input', (e) => {
            if (e.target.classList.contains('option-name-input')) {
                renderVariantsSection();
            }
        });

        addEditProductForm.addEventListener('submit', (e) => {
            e.preventDefault();
            showLoading();

            const id = editProductIdInput.value || generateId();
            const name = newProductNameInput.value.trim();
            const basePrice = parseFloat(newProductBasePriceInput.value);
            const image = newProductImageInput.value.trim();
            const description = newProductDescriptionInput.value.trim();
            const reviewsCount = parseInt(newProductReviewsInput.value) || 0;

            if (!name || isNaN(basePrice) || basePrice < 0) {
                showMessage('Vui lòng điền đầy đủ và đúng định dạng Tên sản phẩm và Giá cơ bản!', 'error');
                hideLoading();
                return;
            }

            const colors = Array.from(colorOptionsContainer.children).map(div => ({
                name: div.querySelector('.option-name-input').value.trim(),
                priceImpact: parseFloat(div.querySelector('.option-price-impact-input').value) || 0,
                display_image: div.querySelector('.option-display-image-input')?.value.trim() || ''
            })).filter(c => c.name);

            const storages = Array.from(storageOptionsContainer.children).map(div => ({
                name: div.querySelector('.option-name-input').value.trim(),
                priceImpact: parseFloat(div.querySelector('.option-price-impact-input').value) || 0
            })).filter(s => s.name);

            const variants = Array.from(variantsContainer.children).map(div => ({
                color: div.querySelector('.variant-color-select').value,
                storage: div.querySelector('.variant-storage-select').value,
                quantity: parseInt(div.querySelector('.variant-quantity-input').value) || 0,
                priceImpact: parseFloat(div.querySelector('.variant-price-impact-input').value) || 0
            })).filter(v => v.color || v.storage);

            const newProduct = { id, name, basePrice, image, description, colors, storages, variants, sold: 0, reviewsCount };

            const existingProductIndex = shopData.products.findIndex(p => p.id === id);
            if (existingProductIndex > -1) {
                shopData.products[existingProductIndex] = newProduct;
                showMessage('Cập nhật sản phẩm thành công!', 'success');
            } else {
                shopData.products.push(newProduct);
                showMessage('Thêm sản phẩm thành công!', 'success');
            }
            saveData();
            renderProductManagementList();
            renderProducts();
            resetAddEditProductForm();
            hideLoading();
        });

        function renderProductManagementList() {
            productManagementList.innerHTML = '';
            if (shopData.products.length === 0) {
                productManagementList.innerHTML = '<p class="text-gray-500 italic">Chưa có sản phẩm nào để quản lý.</p>';
                return;
            }
            shopData.products.forEach(product => {
                const productItem = document.createElement('div');
                productItem.className = 'management-item-card flex justify-between items-center bg-gray-100 p-4 rounded-lg shadow-sm';
                productItem.innerHTML = `
                    <div class="flex items-center gap-4">
                        <img src="${product.image}" onerror="this.onerror=null;this.src='https://placehold.co/50x50/cccccc/333333?text=No';" class="w-12 h-12 object-cover rounded-md">
                        <span class="font-semibold text-gray-800">${product.name} (${formatCurrency(product.basePrice)})</span>
                    </div>
                    <div>
                        <button data-product-id="${product.id}" class="edit-product-btn text-blue-500 hover:text-blue-700 mr-2"><i class="fas fa-edit"></i> Sửa</button>
                        <button data-product-id="${product.id}" class="duplicate-product-btn text-purple-500 hover:text-purple-700 mr-2"><i class="fas fa-copy"></i> Sao chép</button>
                        <button data-product-id="${product.id}" class="delete-product-btn text-red-500 hover:text-red-700"><i class="fas fa-trash-alt"></i> Xóa</button>
                    </div>
                `;
                productManagementList.appendChild(productItem);
            });

            document.querySelectorAll('.edit-product-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const productId = e.currentTarget.dataset.productId;
                    const productToEdit = shopData.products.find(p => p.id === productId);
                    if (productToEdit) {
                        editProduct(productToEdit);
                    }
                });
            });

            document.querySelectorAll('.duplicate-product-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const productId = e.currentTarget.dataset.productId;
                    const productToDuplicate = shopData.products.find(p => p.id === productId);
                    if (productToDuplicate) {
                        duplicateProduct(productToDuplicate);
                    }
                });
            });

            document.querySelectorAll('.delete-product-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const productId = e.currentTarget.dataset.productId;
                    showConfirmModal('Bạn có chắc chắn muốn xóa sản phẩm này?', () => {
                        shopData.products = shopData.products.filter(p => p.id !== productId);
                        saveData();
                        renderProductManagementList();
                        renderProducts();
                        showMessage('Sản phẩm đã được xóa.', 'success');
                    });
                });
            });
        }

        function editProduct(product) {
            addEditProductTitle.textContent = 'Chỉnh sửa Mặt Hàng';
            submitProductBtn.textContent = 'Lưu Thay Đổi';
            cancelEditBtn.classList.remove('hidden');

            editProductIdInput.value = product.id;
            newProductNameInput.value = product.name;
            newProductBasePriceInput.value = product.basePrice;
            newProductImageInput.value = product.image;
            newProductDescriptionInput.value = product.description;
            newProductReviewsInput.value = product.reviewsCount || 0;

            colorOptionsContainer.innerHTML = '';
            storageOptionsContainer.innerHTML = '';
            variantsContainer.innerHTML = '';

            product.colors.forEach(color => renderOptionInput('color-options-container', 'color', color.name, color.priceImpact, color.display_image || ''));
            product.storages.forEach(storage => renderOptionInput('storage-options-container', 'storage', storage.name, storage.priceImpact));
            product.variants.forEach(variant => renderVariantInput(variant.color, variant.storage, variant.quantity, variant.priceImpact));
        }

        function duplicateProduct(product) {
            const duplicatedProduct = JSON.parse(JSON.stringify(product));
            duplicatedProduct.id = generateId();
            duplicatedProduct.name = `Bản sao của ${duplicatedProduct.name}`;

            editProduct(duplicatedProduct);
            addEditProductTitle.textContent = 'Thêm Mặt Hàng Mới (Sao chép)';
            submitProductBtn.textContent = 'Tạo Sản Phẩm Mới';
        }

        cancelEditBtn.addEventListener('click', resetAddEditProductForm);

        function resetAddEditProductForm() {
            addEditProductTitle.textContent = 'Thêm Mặt Hàng Mới';
            submitProductBtn.textContent = 'Thêm Sản Phẩm';
            cancelEditBtn.classList.add('hidden');
            addEditProductForm.reset();
            editProductIdInput.value = '';
            newProductReviewsInput.value = 0;
            colorOptionsContainer.innerHTML = '';
            storageOptionsContainer.innerHTML = '';
            variantsContainer.innerHTML = '';
        }

        function updateCartCount() {
            cartCountSpan.textContent = shopData.cart.length;
        }

        addToCartDetailBtn.addEventListener('click', () => {
            if (!currentSelectedProduct) {
                showMessage('Vui lòng chọn một sản phẩm trước khi thêm vào giỏ hàng.', 'error');
                return;
            }
            if (addToCartDetailBtn.disabled) {
                showMessage('Sản phẩm này hiện đang hết hàng hoặc biến thể không tồn tại!', 'error');
                return;
            }

            const cartItem = {
                cartItemId: generateId(),
                productId: currentSelectedProduct.id,
                productName: currentSelectedProduct.name,
                productImage: currentSelectedOptions.color && currentSelectedOptions.color.display_image ? currentSelectedOptions.color.display_image : currentSelectedProduct.image,
                options: { ...currentSelectedOptions },
                basePrice: currentSelectedProduct.basePrice,
                priceAfterOptions: currentCalculatedPrice,
                voucherApplied: currentAppliedVoucher ? currentAppliedVoucher.code : null,
                priceAfterVoucher: currentCalculatedPrice,
                shippingCostApplied: (currentAppliedVoucher && currentAppliedVoucher.value === 'freeship') ? 0 : fixedShippingCost,
                quantity: 1
            };

            shopData.cart.push(cartItem);
            saveData();
            updateCartCount();
            showMessage('Sản phẩm đã được thêm vào giỏ hàng!', 'success');
            closeModal(productDetailModal);
        });

        function renderCart(searchTerm = '') {
            cartItemsList.innerHTML = '';
            let totalCartAmount = 0;
            const lowerCaseSearchTerm = searchTerm.toLowerCase();

            const filteredCartItems = shopData.cart.filter(item => 
                item.productName.toLowerCase().includes(lowerCaseSearchTerm) ||
                (item.options.color && item.options.color.value && item.options.color.value.toLowerCase().includes(lowerCaseSearchTerm)) ||
                (item.options.storage && item.options.storage.value && item.options.storage.value.toLowerCase().includes(lowerCaseSearchTerm))
            );

            if (filteredCartItems.length === 0) {
                cartItemsList.innerHTML = '<p class="text-gray-500 italic text-center">Giỏ hàng trống.</p>';
                cartTotalAmountSpan.textContent = formatCurrency(0);
                buyAllCartBtn.disabled = true;
                buyAllCartBtn.classList.add('bg-gray-400', 'hover:bg-gray-400');
                buyAllCartBtn.classList.remove('bg-blue-600', 'hover:bg-blue-700');
                return;
            }

            filteredCartItems.forEach(item => {
                const optionsDisplay = Object.entries(item.options)
                    .filter(([key, val]) => val !== null)
                    .map(([key, val]) => `${key === 'color' ? 'Màu' : 'Dung lượng'}: ${val.value}`)
                    .join(', ');

                const cartItemCard = document.createElement('div');
                cartItemCard.className = 'flex items-center bg-white rounded-lg shadow-md p-4 mb-3';
                cartItemCard.innerHTML = `
                    <img src="${item.productImage}" onerror="this.onerror=null;this.src='https://placehold.co/80x80/cccccc/333333?text=No';" class="w-20 h-20 object-cover rounded-md mr-4">
                    <div class="flex-1">
                        <p class="font-semibold text-gray-800">${item.productName}</p>
                        ${optionsDisplay ? `<p class="text-sm text-gray-600">${optionsDisplay}</p>` : ''}
                        <p class="text-sm text-gray-700">Giá: <span class="font-bold">${formatCurrency(item.priceAfterVoucher)}</span></p>
                        <p class="text-sm text-gray-700">Phí ship: <span class="font-bold">${formatCurrency(item.shippingCostApplied)}</span></p>
                        ${item.voucherApplied ? `<p class="text-xs text-green-600">Voucher: ${item.voucherApplied}</p>` : ''}
                    </div>
                    <div class="flex flex-col space-y-2 ml-4">
                        <button data-cart-item-id="${item.cartItemId}" class="buy-now-cart-item-btn bg-green-500 hover:bg-green-600 text-white py-2 px-3 rounded-lg text-sm font-medium transition-colors duration-200"><i class="fas fa-dollar-sign mr-1"></i>Mua Ngay</button>
                        <button data-cart-item-id="${item.cartItemId}" class="remove-from-cart-btn bg-red-500 hover:bg-red-600 text-white py-2 px-3 rounded-lg text-sm font-medium transition-colors duration-200"><i class="fas fa-trash-alt mr-1"></i>Xóa</button>
                    </div>
                `;
                cartItemsList.appendChild(cartItemCard);
                totalCartAmount += item.priceAfterVoucher + item.shippingCostApplied;
            });

            cartTotalAmountSpan.textContent = formatCurrency(totalCartAmount);
            buyAllCartBtn.disabled = false;
            buyAllCartBtn.classList.remove('bg-gray-400', 'hover:bg-gray-400');
            buyAllCartBtn.classList.add('bg-blue-600', 'hover:bg-blue-700');

            document.querySelectorAll('.buy-now-cart-item-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const cartItemId = e.currentTarget.dataset.cartItemId;
                    const itemToBuy = shopData.cart.find(cartItem => cartItem.cartItemId === cartItemId);
                    if (itemToBuy) {
                        productsToOrder = [itemToBuy];
                        isBuyNowFlow = false; 
                        openOrderCreationModal();
                        shopData.cart = shopData.cart.filter(cartItem => cartItem.cartItemId !== cartItemId);
                        saveData();
                        updateCartCount();
                        renderCart();
                    }
                });
            });

            document.querySelectorAll('.remove-from-cart-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const cartItemId = e.currentTarget.dataset.cartItemId;
                    shopData.cart = shopData.cart.filter(cartItem => cartItem.cartItemId !== cartItemId);
                    saveData();
                    updateCartCount();
                    renderCart();
                    showMessage('Sản phẩm đã được xóa khỏi giỏ hàng.', 'info');
                });
            });
        }

        searchCartItemsInput.addEventListener('input', () => {
            renderCart(searchCartItemsInput.value.trim());
        });
        clearSearchCartItemsBtn.addEventListener('click', () => {
            searchCartItemsInput.value = '';
            renderCart();
        });

        buyNowDetailBtn.addEventListener('click', () => {
            if (!currentSelectedProduct) {
                showMessage('Vui lòng chọn một sản phẩm trước khi mua ngay.', 'error');
                return;
            }
            if (buyNowDetailBtn.disabled) {
                showMessage('Sản phẩm này hiện đang hết hàng hoặc biến thể không tồn tại!', 'error');
                return;
            }
            productsToOrder = [{
                productId: currentSelectedProduct.id,
                productName: currentSelectedProduct.name,
                productImage: currentSelectedOptions.color && currentSelectedOptions.color.display_image ? currentSelectedOptions.color.display_image : currentSelectedProduct.image,
                options: { ...currentSelectedOptions },
                basePrice: currentSelectedProduct.basePrice,
                priceAfterOptions: currentCalculatedPrice,
                voucherApplied: currentAppliedVoucher ? currentAppliedVoucher.code : null,
                priceAfterVoucher: currentCalculatedPrice,
                shippingCostApplied: (currentAppliedVoucher && currentAppliedVoucher.value === 'freeship') ? 0 : fixedShippingCost,
                quantity: 1
            }];
            isBuyNowFlow = true; 
            openOrderCreationModal();
        });

        buyAllCartBtn.addEventListener('click', () => {
            if (shopData.cart.length === 0) {
                showMessage('Giỏ hàng của bạn đang trống!', 'error');
                return;
            }
            productsToOrder = [...shopData.cart];
            isBuyNowFlow = false; 
            openOrderCreationModal();
        });

        function openOrderCreationModal() {
            const newOrderId = generateId().substring(0, 8).toUpperCase();
            orderIdDisplay.textContent = newOrderId;
            
            customerNameInput.value = '';
            customerPhoneInput.value = '';
            customerAddressInput.value = '';
            orderLocationInput.value = '';
            estimatedDeliveryDateInput.value = '';

            customerPhoneInput.classList.add('blurred-content');
            customerAddressInput.classList.add('blurred-content');

            orderProductsSummary.innerHTML = `
                <p class="font-semibold text-gray-800 mb-2">Sản phẩm trong đơn hàng:</p>
            `;
            productsToOrder.forEach(item => {
                const productSummaryDiv = document.createElement('div');
                productSummaryDiv.className = 'flex items-center gap-2 mb-2';
                const optionsSummary = Object.entries(item.options)
                    .filter(([key, val]) => val !== null)
                    .map(([key, val]) => `${key === 'color' ? 'Màu' : 'Dung lượng'}: ${val.value}`)
                    .join(', ');
                productSummaryDiv.innerHTML = `
                    <img src="${item.productImage}" onerror="this.onerror=null;this.src='https://placehold.co/40x40/cccccc/333333?text=No';" class="w-10 h-10 object-cover rounded-md">
                    <span class="text-sm font-medium">${item.productName} ${optionsSummary ? `(${optionsSummary})` : ''} - ${formatCurrency(item.priceAfterVoucher)}</span>
                `;
                orderProductsSummary.appendChild(productSummaryDiv);
            });

            closeModal(cartModal);
            closeModal(productDetailModal);
            openModal(orderCreationModal);
        }

        orderForm.addEventListener('submit', (e) => {
            e.preventDefault();
            showLoading();

            const orderId = orderIdDisplay.textContent;
            const customerName = customerNameInput.value.trim();
            const customerPhone = customerPhoneInput.value.trim();
            const customerAddress = customerAddressInput.value.trim();
            const orderLocation = orderLocationInput.value.trim();
            const estimatedDeliveryDate = estimatedDeliveryDateInput.value.trim();

            if (!customerName || !customerPhone || !customerAddress) {
                showMessage('Vui lòng điền đầy đủ thông tin khách hàng.', 'error');
                hideLoading();
                return;
            }

            productsToOrder.forEach(orderedProduct => {
                const newOrder = {
                    id: generateId().substring(0, 8).toUpperCase(),
                    productId: orderedProduct.productId,
                    productName: orderedProduct.productName,
                    productImage: orderedProduct.productImage,
                    productOptions: orderedProduct.options,
                    totalAmount: orderedProduct.priceAfterVoucher,
                    voucherApplied: orderedProduct.voucherApplied,
                    shippingCost: orderedProduct.shippingCostApplied,
                    customerName: customerName,
                    customerPhone: customerPhone,
                    customerAddress: customerAddress,
                    orderLocation: orderLocation,
                    estimatedDeliveryDate: estimatedDeliveryDate,
                    status: 'created',
                    orderDate: new Date().toISOString(),
                    amountPaid: 0,
                    remainingPayment: orderedProduct.priceAfterVoucher + orderedProduct.shippingCostApplied
                };

                const productIndex = shopData.products.findIndex(p => p.id === orderedProduct.productId);
                if (productIndex !== -1) {
                    const productToUpdate = shopData.products[productIndex];
                    let decreased = false;
                    if (productToUpdate.variants && productToUpdate.variants.length > 0) {
                        const selectedColor = orderedProduct.options.color ? orderedProduct.options.color.value : null;
                        const selectedStorage = orderedProduct.options.storage ? orderedProduct.options.storage.value : null;

                        const matchingVariant = productToUpdate.variants.find(variant =>
                            (variant.color === selectedColor || (!selectedColor && variant.color === null)) &&
                            (variant.storage === selectedStorage || (!selectedStorage && variant.storage === null))
                        );
                        if (matchingVariant && matchingVariant.quantity > 0) {
                            matchingVariant.quantity--;
                            productToUpdate.sold = (productToUpdate.sold || 0) + 1;
                            decreased = true;
                        }
                    } else if (productToUpdate.quantity > 0) {
                        productToUpdate.quantity--;
                        productToUpdate.sold = (productToUpdate.sold || 0) + 1;
                        decreased = true;
                    }
                    if (!decreased) {
                        showMessage(`Cảnh báo: Không thể giảm số lượng cho sản phẩm "${orderedProduct.productName}" (hết hàng hoặc biến thể không tồn tại).`, 'warning');
                    }
                }

                shopData.orders.push(newOrder);
            });

            if (!isBuyNowFlow) {
                shopData.cart = [];
            }
            saveData();
            
            renderProducts();
            renderProductManagementList();
            updateCartCount();
            
            renderOrders('created');
            renderOrders('shipping');
            renderOrders('delivered');

            hideLoading();
            showMessage(`Đơn hàng đã được tạo thành công!`, 'success');
            closeModal(orderCreationModal);
        });

        function renderOrders(statusFilter = 'all') {
            const createdList = document.getElementById('created-orders-list');
            const shippingList = document.getElementById('shipping-orders-list');
            const deliveredList = document.getElementById('delivered-orders-list');

            if (statusFilter === 'created' || statusFilter === 'all') createdList.innerHTML = '';
            if (statusFilter === 'shipping' || statusFilter === 'all') shippingList.innerHTML = '';
            if (statusFilter === 'delivered' || statusFilter === 'all') deliveredList.innerHTML = '';

            const filteredOrders = shopData.orders.filter(order => {
                if (statusFilter === 'all') return true;
                if (statusFilter === 'created' && order.status === 'created') return true;
                if (statusFilter === 'shipping' && order.status === 'shipping') return true;
                if (statusFilter === 'delivered' && order.status === 'delivered') return true;
                if (statusFilter === 'cancelled' && order.status === 'cancelled') return true;
                return false;
            });

            if (filteredOrders.length === 0) {
                const message = '<p class="text-gray-500 italic">Chưa có đơn hàng nào trong mục này.</p>';
                if (statusFilter === 'created') createdList.innerHTML = message;
                if (statusFilter === 'shipping') shippingList.innerHTML = message;
                if (statusFilter === 'delivered') deliveredList.innerHTML = message;
                return;
            }

            filteredOrders.forEach(order => {
                const orderCard = document.createElement('div');
                orderCard.className = 'order-item-card bg-white rounded-xl shadow-md p-4 mb-4 relative';
                
                const product = shopData.products.find(p => p.id === order.productId);
                const productImage = product ? order.productImage : 'https://placehold.co/50x50/cccccc/333333?text=No';
                const optionsDisplay = Object.entries(order.productOptions)
                    .filter(([key, val]) => val !== null)
                    .map(([key, val]) => `${key === 'color' ? 'Màu' : 'Dung lượng'}: ${val.value}`)
                    .join(', ');

                orderCard.innerHTML = `
                    <div class="flex justify-between items-start mb-3">
                        <div>
                            <h4 class="text-xl font-semibold text-gray-900 mb-1">Đơn hàng #${order.id}</h4>
                            <p class="text-gray-700 text-sm">Ngày đặt: ${new Date(order.orderDate).toLocaleDateString()} ${new Date(order.orderDate).toLocaleTimeString()}</p>
                            <p class="text-lg font-bold text-blue-700">Tổng tiền: ${formatCurrency(order.totalAmount)}</p>
                            <p class="text-gray-700 text-sm">Phí vận chuyển: <span class="font-bold">${formatCurrency(order.shippingCost)}</span></p>
                            ${order.amountPaid > 0 ? `<p class="text-sm font-semibold text-green-700">Đã thanh toán trước: ${formatCurrency(order.amountPaid)}</p>` : ''}
                            ${order.remainingPayment !== undefined && order.remainingPayment !== (order.totalAmount + order.shippingCost) ? `<p class="text-sm font-semibold text-red-700">Thanh toán khi nhận hàng: ${formatCurrency(order.remainingPayment)}</p>` : ''}
                        </div>
                        <span class="text-sm font-medium px-3 py-1 rounded-full ${order.status === 'created' ? 'bg-yellow-200 text-yellow-800' : order.status === 'shipping' ? 'bg-blue-200 text-blue-800' : order.status === 'delivered' ? 'bg-green-200 text-green-800' : 'bg-red-200 text-red-800'}">
                            ${order.status === 'created' ? 'Đã tạo' : order.status === 'shipping' ? 'Đang vận chuyển' : order.status === 'delivered' ? 'Đã giao' : 'Đã hủy'}
                        </span>
                    </div>
                    
                    <div class="flex items-center gap-4 mb-3 border-t border-b border-gray-200 py-3">
                        <img src="${productImage}" onerror="this.onerror=null;this.src='https://placehold.co/80x80/cccccc/333333?text=No';" alt="${order.productName}" class="w-20 h-20 object-cover rounded-lg shadow-sm">
                        <div>
                            <p class="font-semibold text-gray-800">${order.productName}</p>
                            ${optionsDisplay ? `<p class="text-sm text-gray-600">${optionsDisplay}</p>` : ''}
                            <p class="text-sm text-gray-600">Số lượng: 1</p>
                            ${order.voucherApplied ? `<p class="text-xs text-green-600">Voucher: ${order.voucherApplied}</p>` : ''}
                        </div>
                    </div>

                    <div class="mb-3 text-gray-800">
                        <p class="font-semibold">Thông tin khách hàng:</p>
                        <p class="text-sm">Tên: <span class="customer-name-display">${order.customerName}</span></p>
                        <div class="relative inline-block">
                            <p class="text-sm">SĐT: <span class="customer-phone-display blurred-content" data-original="${order.customerPhone}">${order.customerPhone}</span></p>
                            <button type="button" class="toggle-visibility-btn absolute top-0.5 right-0 bg-transparent border-none text-gray-500 hover:text-gray-700" data-target-class="customer-phone-display"><i class="fas fa-eye"></i></button>
                        </div>
                        <div class="relative inline-block w-full">
                            <p class="text-sm">Địa chỉ: <span class="customer-address-display blurred-content" data-original="${order.customerAddress}">${order.customerAddress}</span></p>
                            <button type="button" class="toggle-visibility-btn absolute top-0.5 right-0 bg-transparent border-none text-gray-500 hover:text-gray-700" data-target-class="customer-address-display"><i class="fas fa-eye"></i></button>
                        </div>
                        ${order.status === 'shipping' ? `
                            <p class="text-sm mt-2">Vị trí: <span class="font-medium">${order.orderLocation || 'Chưa cập nhật'}</span></p>
                            <p class="text-sm">Dự kiến nhận: <span class="font-medium">${order.estimatedDeliveryDate ? new Date(order.estimatedDeliveryDate).toLocaleDateString() : 'Chưa cập nhật'}</span></p>
                            <button data-order-id="${order.id}" class="edit-shipping-details-btn mt-2 bg-yellow-500 hover:bg-yellow-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors duration-200">
                                <i class="fas fa-edit mr-1"></i>Chi tiết vận chuyển
                            </button>
                        ` : ''}
                    </div>
                    
                    <div class="flex justify-end gap-2 mt-4">
                        ${order.status === 'created' ? `
                            <button data-order-id="${order.id}" class="pay-voucher-btn bg-purple-500 hover:bg-purple-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors duration-200">
                                <i class="fas fa-qrcode mr-1"></i>Thanh toán Voucher (VAT)
                            </button>
                            <button data-order-id="${order.id}" data-status="shipping" class="update-status-btn bg-blue-500 hover:bg-blue-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors duration-200">
                                Bắt đầu Vận chuyển
                            </button>
                            <button data-order-id="${order.id}" data-status="cancelled" class="cancel-order-btn bg-red-500 hover:bg-red-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors duration-200">
                                Hủy đơn hàng
                            </button>
                        ` : ''}
                        ${order.status === 'shipping' ? `
                            <button data-order-id="${order.id}" data-status="delivered" class="update-status-btn bg-green-500 hover:bg-green-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors duration-200">
                                Đã giao hàng
                            </button>
                        ` : ''}
                        ${order.status === 'delivered' ? `
                            <button data-order-id="${order.id}" class="delete-delivered-order-btn bg-red-500 hover:bg-red-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors duration-200">
                                Xóa đơn hàng
                            </button>
                        ` : ''}
                    </div>
                `;
                if (order.status === 'created') createdList.appendChild(orderCard);
                else if (order.status === 'shipping') shippingList.appendChild(orderCard);
                else if (order.status === 'delivered') deliveredList.appendChild(orderCard);
            });

            document.querySelectorAll('.order-item-card .toggle-visibility-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const targetClass = this.dataset.targetClass;
                    const targetSpan = this.closest('.relative-container, div').querySelector(`.${targetClass}`);
                    
                    if (targetSpan) {
                        if (targetSpan.classList.contains('blurred-content')) {
                            targetSpan.classList.remove('blurred-content');
                            this.innerHTML = '<i class="fas fa-eye-slash"></i>';
                        } else {
                            targetSpan.classList.add('blurred-content');
                            this.innerHTML = '<i class="fas fa-eye"></i>';
                        }
                    }
                });
            });

            document.querySelectorAll('.update-status-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const orderId = e.currentTarget.dataset.orderId;
                    const newStatus = e.currentTarget.dataset.status;
                    updateOrderStatus(orderId, newStatus);
                });
            });

            document.querySelectorAll('.cancel-order-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const orderId = e.currentTarget.dataset.orderId;
                    showConfirmModal(`Bạn có chắc chắn muốn hủy đơn hàng #${orderId}?`, () => {
                        updateOrderStatus(orderId, 'cancelled');
                    });
                });
            });

            document.querySelectorAll('.edit-shipping-details-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const orderId = e.currentTarget.dataset.orderId;
                    const orderToEdit = shopData.orders.find(order => order.id === orderId);
                    if (orderToEdit) {
                        editShippingOrder(orderToEdit);
                    }
                });
            });

            document.querySelectorAll('.delete-delivered-order-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const orderId = e.currentTarget.dataset.orderId;
                    showConfirmModal(`Bạn có chắc chắn muốn xóa đơn hàng #${orderId} khỏi lịch sử?`, () => {
                        deleteOrder(orderId);
                    });
                });
            });

            document.querySelectorAll('.pay-voucher-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const orderId = e.currentTarget.dataset.orderId;
                    const orderToPay = shopData.orders.find(order => order.id === orderId);
                    if (orderToPay) {
                        openPaymentVoucherModal(orderToPay);
                    }
                });
            });
        }

        function editShippingOrder(order) {
            editShippingOrderIdDisplay.textContent = order.id;
            editShippingOrderHiddenId.value = order.id;
            editOrderLocationInput.value = order.orderLocation || '';
            editEstimatedDeliveryDateInput.value = order.estimatedDeliveryDate || '';
            openModal(editShippingOrderModal);
        }

        editShippingOrderForm.addEventListener('submit', (e) => {
            e.preventDefault();
            showLoading();
            const orderId = editShippingOrderHiddenId.value;
            const newLocation = editOrderLocationInput.value.trim();
            const newEstimatedDate = editEstimatedDeliveryDateInput.value.trim();

            const orderIndex = shopData.orders.findIndex(order => order.id === orderId);
            if (orderIndex > -1) {
                shopData.orders[orderIndex].orderLocation = newLocation;
                shopData.orders[orderIndex].estimatedDeliveryDate = newEstimatedDate;
                saveData();
                renderOrders('shipping');
                hideLoading();
                showMessage('Thông tin vận chuyển đã được cập nhật!', 'success');
                closeModal(editShippingOrderModal);
            } else {
                hideLoading();
                showMessage('Không tìm thấy đơn hàng để cập nhật.', 'error');
            }
        });

        function updateOrderStatus(orderId, newStatus) {
            const orderIndex = shopData.orders.findIndex(order => order.id === orderId);
            if (orderIndex > -1) {
                const order = shopData.orders[orderIndex];
                if (newStatus === 'cancelled') {
                    const productIndex = shopData.products.findIndex(p => p.id === order.productId);
                    if (productIndex !== -1) {
                        const productToUpdate = shopData.products[productIndex];
                        let increased = false;
                        if (productToUpdate.variants && productToUpdate.variants.length > 0) {
                            const selectedColor = order.productOptions.color ? order.productOptions.color.value : null;
                            const selectedStorage = order.productOptions.storage ? order.productOptions.storage.value : null;
                            const matchingVariant = productToUpdate.variants.find(variant =>
                                (variant.color === selectedColor || (!selectedColor && variant.color === null)) &&
                                (variant.storage === selectedStorage || (!selectedStorage && variant.storage === null))
                            );
                            if (matchingVariant) {
                                matchingVariant.quantity++;
                                productToUpdate.sold = Math.max(0, (productToUpdate.sold || 0) - 1);
                                increased = true;
                            }
                        } else {
                            productToUpdate.quantity = (productToUpdate.quantity || 0) + 1;
                            productToUpdate.sold = Math.max(0, (productToUpdate.sold || 0) - 1);
                            increased = true;
                        }
                        if (!increased) {
                            showMessage(`Cảnh báo: Không thể hoàn trả số lượng cho sản phẩm "${order.productName}".`, 'warning');
                        }
                    }
                }
                shopData.orders[orderIndex].status = newStatus;
                saveData();
                renderOrders('created');
                renderOrders('shipping');
                renderOrders('delivered');
                renderProductManagementList(); 
                showMessage(`Đơn hàng #${orderId} đã chuyển sang trạng thái "${newStatus === 'created' ? 'Đã tạo' : newStatus === 'shipping' ? 'Đang vận chuyển' : newStatus === 'delivered' ? 'Đã giao' : 'Đã hủy'}".`, 'success');
            }
        }

        function deleteOrder(orderId) {
            shopData.orders = shopData.orders.filter(order => order.id !== orderId);
            saveData();
            renderOrders('delivered');
            showMessage(`Đơn hàng #${orderId} đã được xóa.`, 'success');
        }

        document.getElementById('search-created-orders').addEventListener('input', (e) => {
            filterOrdersInList(e.target.value, 'created', 'created-orders-list');
        });
        document.getElementById('clear-search-created-orders').addEventListener('click', () => {
            document.getElementById('search-created-orders').value = '';
            filterOrdersInList('', 'created', 'created-orders-list');
        });

        document.getElementById('search-shipping-orders').addEventListener('input', (e) => {
            filterOrdersInList(e.target.value, 'shipping', 'shipping-orders-list');
        });
        document.getElementById('clear-search-shipping-orders').addEventListener('click', () => {
            document.getElementById('search-shipping-orders').value = '';
            filterOrdersInList('', 'shipping', 'shipping-orders-list');
        });

        document.getElementById('search-delivered-orders').addEventListener('input', (e) => {
            filterOrdersInList(e.target.value, 'delivered', 'delivered-orders-list');
        });
        document.getElementById('clear-search-delivered-orders').addEventListener('click', () => {
            document.getElementById('search-delivered-orders').value = '';
            filterOrdersInList('', 'delivered', 'delivered-orders-list');
        });

        function filterOrdersInList(searchText, status, listId) {
            const listElement = document.getElementById(listId);
            const items = listElement.querySelectorAll('.order-item-card');
            const lowerCaseSearchText = searchText.toLowerCase();

            items.forEach(item => {
                const orderId = item.querySelector('h4').textContent.toLowerCase();
                const customerName = item.querySelector('.customer-name-display').textContent.toLowerCase();
                const customerPhone = item.querySelector('.customer-phone-display').dataset.original.toLowerCase();
                const customerAddress = item.querySelector('.customer-address-display').dataset.original.toLowerCase();

                if (orderId.includes(lowerCaseSearchText) ||
                    customerName.includes(lowerCaseSearchText) ||
                    customerPhone.includes(lowerCaseSearchText) ||
                    customerAddress.includes(lowerCaseSearchText)) {
                    item.style.display = 'block';
                } else {
                    item.style.display = 'none';
                }
            });
        }

        generateReportBtn.addEventListener('click', () => {
            showLoading();
            const startDate = reportStartDateInput.value ? new Date(reportStartDateInput.value) : null;
            const endDate = reportEndDateInput.value ? new Date(reportEndDateInput.value) : null;

            let totalRevenue = 0;
            let totalOrders = 0;
            const productSales = {};

            shopData.orders.forEach(order => {
                const orderDate = new Date(order.orderDate);
                if ((!startDate || orderDate >= startDate) && (!endDate || orderDate <= endDate)) {
                    if (order.status === 'delivered') {
                        totalRevenue += order.totalAmount - order.shippingCost;
                        totalOrders++;

                        if (productSales[order.productId]) {
                            productSales[order.productId] += (order.quantity || 1);
                        } else {
                            productSales[order.productId] = (order.quantity || 1);
                        }
                    }
                }
            });

            totalRevenueDisplay.textContent = formatCurrency(totalRevenue);
            totalOrdersDisplay.textContent = totalOrders;

            const sortedProductSales = Object.entries(productSales).sort(([,a], [,b]) => b - a);
            topSellingProductsList.innerHTML = '';
            if (sortedProductSales.length === 0) {
                topSellingProductsList.innerHTML = '<li class="italic text-gray-500">Chưa có dữ liệu.</li>';
            } else {
                sortedProductSales.slice(0, 5).forEach(([productId, count]) => {
                    const product = shopData.products.find(p => p.id === productId);
                    if (product) {
                        const listItem = document.createElement('li');
                        listItem.textContent = `${product.name}: ${count} lượt bán`;
                        topSellingProductsList.appendChild(listItem);
                    }
                });
            }
            hideLoading();
            showMessage('Báo cáo đã được tạo.', 'success');
        });

        function showConfirmModal(message, onConfirm) {
            const modal = document.createElement('div');
            modal.className = 'fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-50';
            modal.innerHTML = `
                <div class="bg-white rounded-lg shadow-xl p-6 max-w-sm mx-auto">
                    <p class="text-lg font-semibold mb-4">${message}</p>
                    <div class="flex justify-end space-x-3">
                        <button id="confirm-cancel-btn" class="px-4 py-2 bg-gray-300 text-gray-800 rounded-md hover:bg-gray-400 transition-colors">Hủy</button>
                        <button id="confirm-ok-btn" class="px-4 py-2 bg-red-600 text-white rounded-md hover:bg-red-700 transition-colors">Xác nhận</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);

            const confirmOkBtn = modal.querySelector('#confirm-ok-btn');
            const confirmCancelBtn = modal.querySelector('#confirm-cancel-btn');

            if (confirmOkBtn) {
                confirmOkBtn.addEventListener('click', () => {
                    onConfirm();
                    modal.remove();
                });
            }

            if (confirmCancelBtn) {
                confirmCancelBtn.addEventListener('click', () => {
                    modal.remove();
                });
            }

            modal.addEventListener('click', (e) => {
                if (e.target === modal) {
                    modal.remove();
                }
            });
        }

        function openPaymentVoucherModal(order) {
            currentOrderForPayment = order;

            qrCodeDisplay.src = shopData.bankDetails.qrCodeImage || 'https://placehold.co/200x200/cccccc/333333?text=QR+Code';
            bankNameDisplay.textContent = shopData.bankDetails.bankName || 'Chưa cập nhật';
            accountNumberDisplay.textContent = shopData.bankDetails.accountNumber || 'Chưa cập nhật';
            accountHolderDisplay.textContent = shopData.bankDetails.accountHolder || 'Chưa cập nhật';
            
            const totalOrderAmountWithShipping = order.totalAmount + order.shippingCost;
            paymentModalOrderTotal.textContent = formatCurrency(totalOrderAmountWithShipping);
            
            paymentAmountInput.value = '';
            amountPaidDisplay.textContent = formatCurrency(order.amountPaid || 0);
            remainingPaymentDisplay.textContent = formatCurrency(order.remainingPayment || totalOrderAmountWithShipping);

            openModal(paymentVoucherModal);
        }

        paymentAmountInput.addEventListener('input', () => {
            const inputValue = paymentAmountInput.value.trim();
            if (!currentOrderForPayment) {
                amountPaidDisplay.textContent = formatCurrency(0);
                remainingPaymentDisplay.textContent = formatCurrency(0);
                return;
            }

            const totalOrderAmount = currentOrderForPayment.totalAmount;
            const shippingCost = currentOrderForPayment.shippingCost;
            const orderGrandTotal = totalOrderAmount + shippingCost;
            let amountToPay = 0;

            if (inputValue.endsWith('%')) {
                const percentageValue = parseFloat(inputValue.slice(0, -1));
                if (!isNaN(percentageValue) && percentageValue >= 0) {
                    amountToPay = orderGrandTotal * (percentageValue / 100);
                }
            } else {
                amountToPay = parseFloat(inputValue);
            }

            if (isNaN(amountToPay) || amountToPay < 0) {
                amountToPay = 0;
            }
            
            amountToPay = Math.min(amountToPay, orderGrandTotal);

            const calculatedRemainingPayment = Math.max(0, orderGrandTotal - amountToPay);
            
            amountPaidDisplay.textContent = formatCurrency(amountToPay);
            remainingPaymentDisplay.textContent = formatCurrency(calculatedRemainingPayment);
        });

        confirmPaymentBtn.addEventListener('click', () => {
            if (!currentOrderForPayment) {
                showMessage('Không có đơn hàng nào được chọn để thanh toán.', 'error');
                return;
            }

            const orderIndex = shopData.orders.findIndex(order => order.id === currentOrderForPayment.id);
            if (orderIndex > -1) {
                const inputValue = paymentAmountInput.value.trim();
                let calculatedPaidAmount = 0;
                const totalOrderAmount = shopData.orders[orderIndex].totalAmount;
                const shippingCost = shopData.orders[orderIndex].shippingCost;
                const orderGrandTotal = totalOrderAmount + shippingCost;

                if (inputValue.endsWith('%')) {
                    const percentageValue = parseFloat(inputValue.slice(0, -1));
                    if (!isNaN(percentageValue) && percentageValue >= 0) {
                        calculatedPaidAmount = orderGrandTotal * (percentageValue / 100);
                    }
                } else {
                    calculatedPaidAmount = parseFloat(inputValue);
                }

                if (isNaN(calculatedPaidAmount) || calculatedPaidAmount < 0) {
                    calculatedPaidAmount = 0;
                }
                
                calculatedPaidAmount = Math.min(calculatedPaidAmount, orderGrandTotal);

                shopData.orders[orderIndex].amountPaid = calculatedPaidAmount;
                shopData.orders[orderIndex].remainingPayment = Math.max(0, orderGrandTotal - calculatedPaidAmount);

                shopData.orders[orderIndex].status = 'shipping';

                saveData();
                showMessage(`Đã ghi nhận thanh toán cho đơn hàng #${currentOrderForPayment.id}. Đơn hàng đã được chuyển sang trạng thái "Đang vận chuyển".`, 'success');
                closeModal(paymentVoucherModal);
                renderOrders('created');
                renderOrders('shipping');
            } else {
                showMessage('Không tìm thấy đơn hàng để cập nhật thanh toán.', 'error');
            }
        });


        document.addEventListener('DOMContentLoaded', () => {
            loadShopSettings();
            renderProducts();
            renderOrders('created');
            renderOrders('shipping');
            renderOrders('delivered');
            updateCartCount();
            renderProductManagementList();
            renderVouchersList();
            showSection('product-list-section', showProductsBtn);
        });

        document.getElementById('open-address-in-map-btn').addEventListener('click', () => {
            const address = shopData.address;
            if (address && address !== 'Chưa cập nhật') {
                const encodedAddress = encodeURIComponent(address);
                window.open(`https://www.google.com/maps/search/?api=1&query=${encodedAddress}`, '_blank');
            } else {
                showMessage('Vui lòng cập nhật địa chỉ cửa hàng trong cài đặt trước.', 'info');
            }
        });
    </script>
</body>
</html>
