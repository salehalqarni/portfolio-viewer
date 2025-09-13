<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>عارض المحتوى الرقمي</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #333;
            line-height: 1.6;
        }
        
        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            max-width: 700px;
            width: 90%;
            padding: 40px;
            text-align: center;
        }
        
        .header {
            margin-bottom: 30px;
        }
        
        .title {
            font-size: 28px;
            font-weight: bold;
            color: #667eea;
            margin-bottom: 10px;
        }
        
        .subtitle {
            color: #666;
            font-size: 16px;
        }
        
        .info-card {
            background: #f8f9ff;
            border: 2px solid #e1e5fe;
            border-radius: 15px;
            padding: 25px;
            margin: 20px 0;
        }
        
        .info-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 15px 0;
            padding: 12px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
        }
        
        .info-label {
            font-weight: bold;
            color: #555;
        }
        
        .info-value {
            color: #667eea;
            font-weight: 600;
        }
        
        .error {
            background: #ffebee;
            border: 2px solid #ffcdd2;
            color: #c62828;
        }
        
        .success {
            background: #e8f5e8;
            border: 2px solid #c8e6c9;
            color: #2e7d32;
        }
        
        .contact {
            background: #e3f2fd;
            border: 2px solid #bbdefb;
            color: #1565c0;
        }
        
        .event {
            background: #fff3e0;
            border: 2px solid #ffcc02;
            color: #ef6c00;
        }
        
        .document {
            background: #f3e5f5;
            border: 2px solid #ce93d8;
            color: #7b1fa2;
        }
        
        .icon {
            font-size: 48px;
            margin-bottom: 20px;
        }
        
        .btn {
            background: #667eea;
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s;
            margin: 10px;
            text-decoration: none;
            display: inline-block;
        }
        
        .btn:hover {
            background: #5a6fd8;
            transform: translateY(-2px);
        }
        
        .btn-secondary {
            background: #6c757d;
        }
        
        .btn-secondary:hover {
            background: #545b62;
        }
        
        .btn-download {
            background: #28a745;
        }
        
        .btn-download:hover {
            background: #218838;
        }
        
        .loader {
            border: 3px solid #f3f3f3;
            border-top: 3px solid #667eea;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .footer {
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid #eee;
            color: #666;
            font-size: 14px;
        }
        
        .url-display {
            background: #f1f3f4;
            padding: 10px;
            border-radius: 8px;
            font-family: monospace;
            font-size: 12px;
            color: #333;
            word-break: break-all;
            margin: 10px 0;
        }
        
        .qr-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
        }
        
        .content-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="title">عارض المحتوى الرقمي</div>
            <div class="subtitle">عرض المعلومات من QR Code</div>
        </div>
        
        <div id="content">
            <div class="loader"></div>
            <p>جاري تحليل البيانات...</p>
        </div>
        
        <div class="footer">
            <p>عارض عام للمحتوى الرقمي المشفر في QR Code</p>
        </div>
    </div>

    <script>
        function getUrlParameter(name) {
            const urlParams = new URLSearchParams(window.location.search);
            return urlParams.get(name);
        }
        
        function decodeData(encodedData) {
            try {
                let decodedJson;
                try {
                    decodedJson = atob(encodedData);
                } catch (e) {
                    encodedData = encodedData.replace(/-/g, '+').replace(/_/g, '/');
                    while (encodedData.length % 4) {
                        encodedData += '=';
                    }
                    decodedJson = atob(encodedData);
                }
                return JSON.parse(decodedJson);
            } catch (e) {
                console.error('خطأ في فك التشفير:', e);
                return null;
            }
        }
        
        function displayContent(data) {
            const content = document.getElementById('content');
            
            if (!data) {
                content.innerHTML = `
                    <div class="info-card error">
                        <div class="icon">❌</div>
                        <h3>خطأ في البيانات</h3>
                        <p>لم يتم العثور على بيانات صالحة في الرمز المسروح</p>
                        <div class="url-display">
                            الرابط الحالي: ${window.location.href}
                        </div>
                        <button class="btn btn-secondary" onclick="showDebugInfo()">معلومات التشخيص</button>
                    </div>
                `;
                return;
            }
            
            // تحديد نوع المحتوى وعرضه
            switch(data.type) {
                case 'portfolio':
                case 'teacher':
                case 'adminAssistant':
                case 'kindergarten':
                    displayPortfolioData(data);
                    break;
                case 'contact':
                    displayContactData(data);
                    break;
                case 'event':
                    displayEventData(data);
                    break;
                case 'document':
                    displayDocumentData(data);
                    break;
                case 'business':
                    displayBusinessData(data);
                    break;
                case 'menu':
                    displayMenuData(data);
                    break;
                default:
                    displayGenericData(data);
            }
        }
        
        function displayPortfolioData(data) {
            const content = document.getElementById('content');
            const date = new Date(data.date);
            
            content.innerHTML = `
                <div class="info-card success document">
                    <div class="icon">📄</div>
                    <h3>ملف الأداء المهني</h3>
                    
                    <div class="info-item">
                        <span class="info-label">اسم المعلم:</span>
                        <span class="info-value">${data.name || 'غير محدد'}</span>
                    </div>
                    
                    <div class="info-item">
                        <span class="info-label">التخصص:</span>
                        <span class="info-value">${data.spec || 'غير محدد'}</span>
                    </div>
                    
                    <div class="info-item">
                        <span class="info-label">نوع المعلم:</span>
                        <span class="info-value">${getTeacherTypeArabic(data.type)}</span>
                    </div>
                    
                    <div class="info-item">
                        <span class="info-label">عدد الصور:</span>
                        <span class="info-value">${data.images || 0} صورة</span>
                    </div>
                    
                    <div class="info-item">
                        <span class="info-label">تاريخ الإنشاء:</span>
                        <span class="info-value">${date.toLocaleDateString('ar-SA')}</span>
                    </div>
                    
                    ${data.pdfUrl ? `<div class="qr-actions">
                        <a href="${data.pdfUrl}" target="_blank" class="btn btn-download">تحميل الملف</a>
                    </div>` : ''}
                </div>
                
                <div class="qr-actions">
                    <button class="btn" onclick="window.close()">إغلاق</button>
                    <button class="btn btn-secondary" onclick="location.reload()">إعادة التحميل</button>
                </div>
            `;
        }
        
        function displayContactData(data) {
            const content = document.getElementById('content');
            
            content.innerHTML = `
                <div class="info-card success contact">
                    <div class="icon">👤</div>
                    <h3>معلومات التواصل</h3>
                    
                    <div class="content-grid">
                        ${data.name ? `<div class="info-item">
                            <span class="info-label">الاسم:</span>
                            <span class="info-value">${data.name}</span>
                        </div>` : ''}
                        
                        ${data.phone ? `<div class="info-item">
                            <span class="info-label">الهاتف:</span>
                            <span class="info-value">${data.phone}</span>
                        </div>` : ''}
                        
                        ${data.email ? `<div class="info-item">
                            <span class="info-label">البريد الإلكتروني:</span>
                            <span class="info-value">${data.email}</span>
                        </div>` : ''}
                        
                        ${data.company ? `<div class="info-item">
                            <span class="info-label">الشركة:</span>
                            <span class="info-value">${data.company}</span>
                        </div>` : ''}
                        
                        ${data.position ? `<div class="info-item">
                            <span class="info-label">المنصب:</span>
                            <span class="info-value">${data.position}</span>
                        </div>` : ''}
                        
                        ${data.website ? `<div class="info-item">
                            <span class="info-label">الموقع الإلكتروني:</span>
                            <span class="info-value"><a href="${data.website}" target="_blank">${data.website}</a></span>
                        </div>` : ''}
                    </div>
                    
                    <div class="qr-actions">
                        ${data.phone ? `<a href="tel:${data.phone}" class="btn">اتصال</a>` : ''}
                        ${data.email ? `<a href="mailto:${data.email}" class="btn">إرسال بريد</a>` : ''}
                        ${data.website ? `<a href="${data.website}" target="_blank" class="btn">زيارة الموقع</a>` : ''}
                    </div>
                </div>
            `;
        }
        
        function displayEventData(data) {
            const content = document.getElementById('content');
            const eventDate = data.date ? new Date(data.date) : null;
            
            content.innerHTML = `
                <div class="info-card success event">
                    <div class="icon">📅</div>
                    <h3>معلومات الفعالية</h3>
                    
                    <div class="content-grid">
                        ${data.title ? `<div class="info-item">
                            <span class="info-label">اسم الفعالية:</span>
                            <span class="info-value">${data.title}</span>
                        </div>` : ''}
                        
                        ${eventDate ? `<div class="info-item">
                            <span class="info-label">التاريخ:</span>
                            <span class="info-value">${eventDate.toLocaleDateString('ar-SA')}</span>
                        </div>` : ''}
                        
                        ${data.time ? `<div class="info-item">
                            <span class="info-label">الوقت:</span>
                            <span class="info-value">${data.time}</span>
                        </div>` : ''}
                        
                        ${data.location ? `<div class="info-item">
                            <span class="info-label">المكان:</span>
                            <span class="info-value">${data.location}</span>
                        </div>` : ''}
                        
                        ${data.organizer ? `<div class="info-item">
                            <span class="info-label">المنظم:</span>
                            <span class="info-value">${data.organizer}</span>
                        </div>` : ''}
                        
                        ${data.description ? `<div class="info-item" style="grid-column: 1/-1;">
                            <span class="info-label">الوصف:</span>
                            <span class="info-value">${data.description}</span>
                        </div>` : ''}
                    </div>
                    
                    <div class="qr-actions">
                        ${data.registrationUrl ? `<a href="${data.registrationUrl}" target="_blank" class="btn">التسجيل</a>` : ''}
                        ${data.mapsUrl ? `<a href="${data.mapsUrl}" target="_blank" class="btn btn-secondary">عرض الخريطة</a>` : ''}
                    </div>
                </div>
            `;
        }
        
        function displayDocumentData(data) {
            const content = document.getElementById('content');
            const createdDate = data.created ? new Date(data.created) : null;
            
            content.innerHTML = `
                <div class="info-card success document">
                    <div class="icon">📄</div>
                    <h3>معلومات المستند</h3>
                    
                    <div class="content-grid">
                        ${data.title ? `<div class="info-item">
                            <span class="info-label">عنوان المستند:</span>
                            <span class="info-value">${data.title}</span>
                        </div>` : ''}
                        
                        ${data.author ? `<div class="info-item">
                            <span class="info-label">المؤلف:</span>
                            <span class="info-value">${data.author}</span>
                        </div>` : ''}
                        
                        ${data.category ? `<div class="info-item">
                            <span class="info-label">الفئة:</span>
                            <span class="info-value">${data.category}</span>
                        </div>` : ''}
                        
                        ${createdDate ? `<div class="info-item">
                            <span class="info-label">تاريخ الإنشاء:</span>
                            <span class="info-value">${createdDate.toLocaleDateString('ar-SA')}</span>
                        </div>` : ''}
                        
                        ${data.size ? `<div class="info-item">
                            <span class="info-label">حجم الملف:</span>
                            <span class="info-value">${data.size}</span>
                        </div>` : ''}
                        
                        ${data.pages ? `<div class="info-item">
                            <span class="info-label">عدد الصفحات:</span>
                            <span class="info-value">${data.pages}</span>
                        </div>` : ''}
                    </div>
                    
                    <div class="qr-actions">
                        ${data.downloadUrl ? `<a href="${data.downloadUrl}" target="_blank" class="btn btn-download">تحميل المستند</a>` : ''}
                        ${data.viewUrl ? `<a href="${data.viewUrl}" target="_blank" class="btn">عرض المستند</a>` : ''}
                    </div>
                </div>
            `;
        }
        
        function displayBusinessData(data) {
            const content = document.getElementById('content');
            
            content.innerHTML = `
                <div class="info-card success contact">
                    <div class="icon">🏢</div>
                    <h3>معلومات النشاط التجاري</h3>
                    
                    <div class="content-grid">
                        ${data.businessName ? `<div class="info-item">
                            <span class="info-label">اسم النشاط:</span>
                            <span class="info-value">${data.businessName}</span>
                        </div>` : ''}
                        
                        ${data.category ? `<div class="info-item">
                            <span class="info-label">النوع:</span>
                            <span class="info-value">${data.category}</span>
                        </div>` : ''}
                        
                        ${data.address ? `<div class="info-item">
                            <span class="info-label">العنوان:</span>
                            <span class="info-value">${data.address}</span>
                        </div>` : ''}
                        
                        ${data.phone ? `<div class="info-item">
                            <span class="info-label">الهاتف:</span>
                            <span class="info-value">${data.phone}</span>
                        </div>` : ''}
                        
                        ${data.hours ? `<div class="info-item">
                            <span class="info-label">ساعات العمل:</span>
                            <span class="info-value">${data.hours}</span>
                        </div>` : ''}
                        
                        ${data.rating ? `<div class="info-item">
                            <span class="info-label">التقييم:</span>
                            <span class="info-value">${data.rating} ⭐</span>
                        </div>` : ''}
                    </div>
                    
                    <div class="qr-actions">
                        ${data.phone ? `<a href="tel:${data.phone}" class="btn">اتصال</a>` : ''}
                        ${data.website ? `<a href="${data.website}" target="_blank" class="btn">الموقع الإلكتروني</a>` : ''}
                        ${data.mapsUrl ? `<a href="${data.mapsUrl}" target="_blank" class="btn btn-secondary">عرض الخريطة</a>` : ''}
                    </div>
                </div>
            `;
        }
        
        function displayMenuData(data) {
            const content = document.getElementById('content');
            
            content.innerHTML = `
                <div class="info-card success event">
                    <div class="icon">🍽️</div>
                    <h3>قائمة الطعام</h3>
                    
                    <div class="content-grid">
                        ${data.restaurantName ? `<div class="info-item">
                            <span class="info-label">اسم المطعم:</span>
                            <span class="info-value">${data.restaurantName}</span>
                        </div>` : ''}
                        
                        ${data.cuisine ? `<div class="info-item">
                            <span class="info-label">نوع المطبخ:</span>
                            <span class="info-value">${data.cuisine}</span>
                        </div>` : ''}
                        
                        ${data.phone ? `<div class="info-item">
                            <span class="info-label">للطلب:</span>
                            <span class="info-value">${data.phone}</span>
                        </div>` : ''}
                        
                        ${data.deliveryTime ? `<div class="info-item">
                            <span class="info-label">وقت التوصيل:</span>
                            <span class="info-value">${data.deliveryTime}</span>
                        </div>` : ''}
                    </div>
                    
                    <div class="qr-actions">
                        ${data.menuUrl ? `<a href="${data.menuUrl}" target="_blank" class="btn btn-download">عرض القائمة الكاملة</a>` : ''}
                        ${data.orderUrl ? `<a href="${data.orderUrl}" target="_blank" class="btn">طلب الآن</a>` : ''}
                        ${data.phone ? `<a href="tel:${data.phone}" class="btn btn-secondary">اتصال للطلب</a>` : ''}
                    </div>
                </div>
            `;
        }
        
        function displayGenericData(data) {
            const content = document.getElementById('content');
            
            content.innerHTML = `
                <div class="info-card success">
                    <div class="icon">ℹ️</div>
                    <h3>معلومات عامة</h3>
                    
                    <div class="content-grid">
                        ${Object.entries(data).map(([key, value]) => {
                            if (typeof value === 'object') return '';
                            return `<div class="info-item">
                                <span class="info-label">${translateKey(key)}:</span>
                                <span class="info-value">${value}</span>
                            </div>`;
                        }).join('')}
                    </div>
                </div>
                
                <div class="qr-actions">
                    <button class="btn" onclick="window.close()">إغلاق</button>
                    <button class="btn btn-secondary" onclick="location.reload()">إعادة التحميل</button>
                </div>
            `;
        }
        
        function getTeacherTypeArabic(type) {
            const types = {
                'teacher': 'معلم',
                'adminAssistant': 'مساعد إداري',
                'kindergarten': 'رياض أطفال',
                'portfolio': 'ملف أداء مهني'
            };
            return types[type] || type || 'غير محدد';
        }
        
        function translateKey(key) {
            const translations = {
                'name': 'الاسم',
                'title': 'العنوان',
                'description': 'الوصف',
                'date': 'التاريخ',
                'time': 'الوقت',
                'location': 'المكان',
                'phone': 'الهاتف',
                'email': 'البريد الإلكتروني',
                'website': 'الموقع',
                'address': 'العنوان',
                'type': 'النوع',
                'category': 'الفئة',
                'version': 'الإصدار'
            };
            return translations[key] || key;
        }
        
        function handleSimpleView() {
            const name = getUrlParameter('name');
            const date = getUrlParameter('date');
            
            if (!name) {
                displayContent(null);
                return;
            }
            
            const simpleData = {
                name: decodeURIComponent(name),
                date: parseInt(date) || Date.now(),
                type: 'portfolio',
                images: 'غير معروف',
                spec: 'غير محدد'
            };
            
            displayContent(simpleData);
        }
        
        function showDebugInfo() {
            const urlParams = new URLSearchParams(window.location.search);
            const allParams = {};
            for (const [key, value] of urlParams) {
                allParams[key] = value;
            }
            
            alert(`معلومات التشخيص:
            
الرابط الكامل: ${window.location.href}

المعاملات الموجودة: ${JSON.stringify(allParams, null, 2)}

أنواع المحتوى المدعومة:
- portfolio: ملف الأداء المهني
- contact: معلومات التواصل
- event: الفعاليات
- document: المستندات
- business: الأنشطة التجارية
- menu: قوائم الطعام`);
        }
        
        // تحميل البيانات عند تحميل الصفحة
        window.onload = function() {
            const encodedData = getUrlParameter('data');
            const isSimpleView = getUrlParameter('name');
            
            if (encodedData) {
                console.log('تم العثور على بيانات مشفرة:', encodedData.substring(0, 50) + '...');
                const contentData = decodeData(encodedData);
                displayContent(contentData);
            } else if (isSimpleView) {
                console.log('استخدام العرض المبسط');
                handleSimpleView();
            } else {
                console.log('لم يتم العثور على بيانات');
                displayContent(null);
            }
        };
        
        // معالجة أخطاء JavaScript
        window.onerror = function(msg, url, line) {
            console.error('خطأ JavaScript:', msg, 'في السطر:', line);
            const content = document.getElementById('content');
            content.innerHTML = `
                <div class="info-card error">
                    <div class="icon">⚠️</div>
                    <h3>خطأ تقني</h3>
                    <p>حدث خطأ في معالجة البيانات: ${msg}</p>
                    <button class="btn" onclick="location.reload()">إعادة المحاولة</button>
                </div>
            `;
        };
    </script>
</body>
</html>
