#   
<!DOCTYPE html>  
<html lang="th">  
<head>  
  <meta charset="UTF-8">  
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">  
  <title>มัน.ตึง.ลำ Cafe' & Rest — ระบบสั่งอาหารประจำโต๊ะ</title>  
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>  
  <link rel="preconnect" href="https://fonts.googleapis.com">  
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>  
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Kanit:wght@300;400;500;600;700&display=swap" rel="stylesheet">  
  <style>  
    body {  
      font-family: 'Kanit', 'Inter', sans-serif;  
      background-color: #141210;  
      color: #f5f2eb;  
      overflow-x: hidden;  
      -webkit-tap-highlight-color: transparent;  
    }  
    .no-scrollbar::-webkit-scrollbar { display: none; }  
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }  
    .chalk-bg { background: radial-gradient(circle at top right, #26221e 0%, #171513 60%, #110f0d 100%); }  
    .wood-bar { background: linear-gradient(180deg, #4a3222 0%, #382417 100%); }  
  </style>  
</head>  
<body class="min-h-screen flex flex-col chalk-bg selection:bg-amber-500 selection:text-stone-950">  
  
  <!-- Top Header Bar -->  
  <header class="sticky top-0 z-30 bg-stone-950/95 backdrop-blur-md border-b border-stone-800 px-3 py-2.5">  
    <div class="max-w-5xl mx-auto flex flex-wrap items-center justify-between gap-2">  
      <div class="flex items-center gap-2.5">  
        <div class="w-10 h-10 rounded-xl bg-amber-500 text-stone-950 flex flex-col items-center justify-center font-bold leading-none shadow-md">  
          <span class="text-xs tracking-tighter">มตล</span>  
          <span class="text-[9px] font-extrabold tracking-widest">CAFE</span>  
        </div>  
        <div>  
          <h1 class="font-bold text-base sm:text-lg text-amber-400 tracking-wide leading-tight">มัน.ตึง.ลำ Cafe' &amp; Rest</h1>  
          <p class="text-xs text-stone-400">เปิดบริการ 10:00 - 20:00 น. • โทร 061-348-5831</p>  
        </div>  
      </div>  
  
      <div class="flex items-center gap-2">  
        <div class="flex items-center gap-1.5 px-3 py-1.5 rounded-xl bg-emerald-950/90 border border-emerald-500/50 text-emerald-300 text-xs font-semibold">  
          <span>📲</span>  
          <span id="currentTableLabel" class="font-extrabold text-amber-300">โต๊ะ 3</span>  
        </div>  
        <button id="btnOpenTableModal" type="button" class="px-2.5 py-1.5 rounded-xl bg-stone-800 hover:bg-stone-700 text-stone-300 text-[11px] font-medium">  
          <span>🔄 เปลี่ยนโต๊ะ</span>  
        </button>  
      </div>  
    </div>  
  </header>  
  
  <!-- Category Filter Bar -->  
  <nav class="sticky top-[57px] z-20 bg-stone-950/90 backdrop-blur-md border-b border-stone-800 py-2.5 px-4">  
    <div id="categoryTabsContainer" class="flex items-center gap-2 overflow-x-auto no-scrollbar"></div>  
  </nav>  
  
  <!-- Main Customer View -->  
  <main class="flex-1 max-w-5xl w-full mx-auto pb-28 px-4 mt-4">  
    <div class="flex items-center justify-between mb-3">  
      <h3 id="activeCategoryTitle" class="text-base sm:text-lg font-bold text-amber-400 flex items-center gap-2">  
        <span>📋</span> รายการเมนูทั้งหมด  
      </h3>  
      <span id="filteredItemCount" class="text-xs text-stone-400">104 รายการ</span>  
    </div>  
  
    <!-- Menu Grid Container -->  
    <div id="menuGridContainer" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3"></div>  
  </main>  
  
  <!-- Bottom Cart Bar -->  
  <div class="fixed bottom-0 inset-x-0 z-30 p-3 bg-gradient-to-t from-stone-950 via-stone-950/95 to-transparent">  
    <div class="max-w-5xl mx-auto flex items-center justify-between gap-3">  
      <button id="btnOpenCartDrawer" type="button" class="flex-1 wood-bar text-white p-3 rounded-2xl shadow-2xl border border-amber-500/40 flex items-center justify-between">  
        <div class="flex items-center gap-3">  
          <div class="w-10 h-10 rounded-xl bg-amber-500 text-stone-950 font-extrabold flex items-center justify-center text-base" id="cartBadgeCount">0</div>  
          <div class="text-left">  
            <div class="text-xs text-amber-200">ตะกร้าอาหาร (<span id="cartBarTableText">โต๊ะ 3</span>)</div>  
            <div class="text-sm font-bold text-white" id="cartBarTotalPrice">0 ฿</div>  
          </div>  
        </div>  
        <span class="px-3 py-2 rounded-xl bg-amber-500 text-stone-950 font-bold text-xs">ดูตะกร้า ▲</span>  
      </button>  
    </div>  
  </div>  
  
  <!-- Cart Modal Drawer -->  
  <div id="cartDrawerModal" class="hidden fixed inset-0 z-50 bg-black/75 backdrop-blur-sm flex items-end sm:items-center justify-center p-0 sm:p-4">  
    <div class="bg-stone-900 border border-stone-700 w-full max-w-lg rounded-t-3xl sm:rounded-3xl overflow-hidden shadow-2xl max-h-[90vh] flex flex-col">  
      <div class="p-4 border-b border-stone-800 flex items-center justify-between bg-stone-950">  
        <h3 class="text-lg font-bold text-amber-400">🛒 ตะกร้าสั่งอาหาร (<span id="cartModalTableName">โต๊ะ 3</span>)</h3>  
        <button id="btnCloseCartModal" type="button" class="w-8 h-8 rounded-full bg-stone-800 text-stone-300 flex items-center justify-center">✕</button>  
      </div>  
      <div id="cartItemsList" class="p-4 overflow-y-auto flex-1 space-y-2.5"></div>  
      <div class="p-4 bg-stone-950 border-t border-stone-800 space-y-3">  
        <div class="flex items-center justify-between text-base font-bold text-white">  
          <span>ยอดรวมทั้งสิ้น</span>  
          <span id="cartModalTotalPrice" class="text-amber-400 text-xl">0 ฿</span>  
        </div>  
        <button id="btnSubmitToLine" type="button" class="w-full py-3.5 rounded-2xl bg-emerald-500 hover:bg-emerald-400 text-stone-950 font-extrabold text-sm sm:text-base shadow-lg transition flex items-center justify-center gap-2">  
          <span>💬 ส่งออเดอร์เข้า LINE ร้านค้าทันที</span>  
        </button>  
      </div>  
    </div>  
  </div>  
  
  <!-- Table Select Modal -->  
  <div id="tableSelectModal" class="hidden fixed inset-0 z-50 bg-black/75 backdrop-blur-sm flex items-center justify-center p-4">  
    <div class="bg-stone-900 border border-stone-700 w-full max-w-sm rounded-3xl overflow-hidden p-4 space-y-3">  
      <div class="flex items-center justify-between border-b border-stone-800 pb-2">  
        <h3 class="font-bold text-amber-400 text-sm">เลือกโต๊ะอาหาร</h3>  
        <button id="btnCloseTableModal" class="text-stone-400">✕</button>  
      </div>  
      <div id="tableOptionsGrid" class="grid grid-cols-3 gap-2"></div>  
    </div>  
  </div>  
  
  <div id="toastBanner" class="hidden fixed top-16 left-1/2 -translate-x-1/2 z-50 px-4 py-2 rounded-xl bg-emerald-500 text-stone-950 font-bold text-xs shadow-2xl"></div>  
  
  <script>  
    const LINE_OA_ID = "@698ygoeo";[span_1](start_span)[span_1](end_span)  
    let state = {  
      currentTable: 'โต๊ะ 3',  
      activeCategory: 'all',  
      cart: []  
    };  
  
    const CATEGORIES = [  
      { id: 'all', name: 'ทั้งหมด', icon: '🍽️' },  
      { id: 'coffee', name: 'กาแฟ & โกโก้', icon: '☕' },  
      { id: 'tea_milk', name: 'ชา & นมสด', icon: '🍵' },  
      { id: 'smoothie_soda', name: 'สมูทตี้ & โซดา', icon: '🍓' },  
      { id: 'beverage', name: 'เครื่องดื่ม & เบียร์', icon: '🍺' },  
      { id: 'yam', name: 'เมนูยำ', icon: '🌶️' },  
      { id: 'fried', name: 'เมนูทอด', icon: '🍗' },  
      { id: 'soup', name: 'ต้ม / แกง', icon: '🍲' },  
      { id: 'appetizer', name: 'กับแกล้ม', icon: '🥗' },  
      { id: 'stirfry', name: 'เมนูผัด', icon: '🥬' },  
      { id: 'singledish', name: 'อาหารจานเดียว', icon: '🍛' }  
    ];  
  
    const MENU_ITEMS = [  
      // Coffee & Cocoa  
      { id: 'c1', cat: 'coffee', name: 'Espresso (เอสเพรสโซ่)', price: 40, emoji: '☕' },  
      { id: 'c2', cat: 'coffee', name: 'Mocca (มอคค่า)', price: 40, emoji: '☕' },  
      { id: 'c3', cat: 'coffee', name: 'Capuccino (คาปูชิโน่)', price: 40, emoji: '☕' },  
      { id: 'c4', cat: 'coffee', name: 'Latte (ลาเต้)', price: 40, emoji: '☕' },  
      { id: 'c5', cat: 'coffee', name: 'Americano (อเมริกาโน่)', price: 40, emoji: '☕' },  
      { id: 'c6', cat: 'coffee', name: 'โกโก้เข้มข้น', price: 35, emoji: '🍫' },  
      // Tea & Milk  
      { id: 't1', cat: 'tea_milk', name: 'ชาเขียว', price: 35, emoji: '🍵' },  
      { id: 't2', cat: 'tea_milk', name: 'ชาเขียวมัจฉะ', price: 40, emoji: '🍵' },  
      { id: 't3', cat: 'tea_milk', name: 'ชาไทย', price: 35, emoji: '🧋' },  
      { id: 't4', cat: 'tea_milk', name: 'ชาพีช', price: 35, emoji: '🍑' },  
      { id: 't5', cat: 'tea_milk', name: 'ชามะนาว', price: 35, emoji: '🍋' },  
      { id: 't6', cat: 'tea_milk', name: 'ชาน้ำผึ้งมะนาว', price: 40, emoji: '🍯' },  
      { id: 't7', cat: 'tea_milk', name: 'นมสด', price: 30, emoji: '🥛' },  
      { id: 't8', cat: 'tea_milk', name: 'นมสดน้ำผึ้ง', price: 35, emoji: '🍯' },  
      { id: 't9', cat: 'tea_milk', name: 'นมชมพู', price: 40, emoji: '🩷' },  
      { id: 't10', cat: 'tea_milk', name: 'นมคาราเมล', price: 40, emoji: '🍮' },  
      // Smoothies & Soda  
      { id: 's1', cat: 'smoothie_soda', name: 'สตอเบอร์รี่สมูทตี้ (ปั่น)', price: 45, emoji: '🍓' },  
      { id: 's2', cat: 'smoothie_soda', name: 'กีวี่สมูทตี้ (ปั่น)', price: 45, emoji: '🥝' },  
      { id: 's3', cat: 'smoothie_soda', name: 'บลูเบอร์รี่สมูทตี้ (ปั่น)', price: 45, emoji: '🫐' },  
      { id: 's4', cat: 'smoothie_soda', name: 'นมสดโอริโอ้ (ปั่น)', price: 45, emoji: '🍪' },  
      { id: 's5', cat: 'smoothie_soda', name: 'สตอเบอร์รี่ อิตาเลี่ยนโซดา', price: 30, emoji: '🍹' },  
      { id: 's6', cat: 'smoothie_soda', name: 'บลูฮาวาย อิตาเลี่ยนโซดา', price: 30, emoji: '🧊' },  
      { id: 's7', cat: 'smoothie_soda', name: 'เสาวรส อิตาเลี่ยนโซดา', price: 30, emoji: '💛' },  
      { id: 's8', cat: 'smoothie_soda', name: 'แอปเปิ้ล อิตาเลี่ยนโซดา', price: 30, emoji: '🍏' },  
      { id: 's9', cat: 'smoothie_soda', name: 'แดงมะนาวโซดา', price: 30, emoji: '🍒' },  
      { id: 's10', cat: 'smoothie_soda', name: 'น้ำผึ้งมะนาวโซดา', price: 30, emoji: '🍋' },  
      { id: 's11', cat: 'smoothie_soda', name: 'องุ่น อิตาเลี่ยนโซดา', price: 30, emoji: '🍇' },  
      { id: 's12', cat: 'smoothie_soda', name: 'กีวี่ อิตาเลี่ยนโซดา', price: 30, emoji: '🥝' },  
      { id: 's13', cat: 'smoothie_soda', name: 'ลิ้นจี่ อิตาเลี่ยนโซดา', price: 30, emoji: '荔' },  
      // Beverage & Beer  
      { id: 'b1', cat: 'beverage', name: 'เบียร์ช้าง (Chang)', price: 65, emoji: '🍺' },  
      { id: 'b2', cat: 'beverage', name: 'เบียร์ลีโอ (Leo)', price: 70, emoji: '🍺' },  
      { id: 'b3', cat: 'beverage', name: 'เบียร์สิงห์ (Singha)', price: 75, emoji: '🍺' },  
      { id: 'b4', cat: 'beverage', name: 'หงส์ทอง (แบน)', price: 190, emoji: '🥃' },  
      { id: 'b5', cat: 'beverage', name: 'แสงโสม (แบน)', price: 200, emoji: '🥃' },  
      { id: 'b6', cat: 'beverage', name: 'รีเจนซี่ (Regency)', price: 470, emoji: '🥃' },  
      { id: 'b7', cat: 'beverage', name: 'โซดา', price: 15, emoji: '🫧' },  
      { id: 'b8', cat: 'beverage', name: 'น้ำเปล่า', price: 10, emoji: '💧' },  
      { id: 'b9', cat: 'beverage', name: 'น้ำอัดลม', price: 20, emoji: '🥤' },  
      { id: 'b10', cat: 'beverage', name: 'น้ำแข็ง (ถังเล็ก)', price: 10, emoji: '🧊' },  
      { id: 'b11', cat: 'beverage', name: 'น้ำแข็ง (ถังใหญ่)', price: 20, emoji: '🪣' },  
      { id: 'b12', cat: 'beverage', name: 'ค่าเปิดเหล้า', price: 30, emoji: '🍾' },  
      // Yam (เมนูยำ)  
      { id: 'y1', cat: 'yam', name: 'ยำหมูยอ', price: 79, emoji: '🌶️' },  
      { id: 'y2', cat: 'yam', name: 'ยำปูอัด', price: 79, emoji: '🦀' },  
      { id: 'y3', cat: 'yam', name: 'ยำแหนม', price: 89, emoji: '🌶️' },  
      { id: 'y4', cat: 'yam', name: 'ยำไข่เยี่ยวม้า', price: 89, emoji: '🥚' },  
      { id: 'y5', cat: 'yam', name: 'ยำวุ้นเส้นหมูสับ', price: 79, emoji: '🍜' },  
      { id: 'y6', cat: 'yam', name: 'ยำวุ้นเส้นทะเล', price: 99, emoji: '🦐' },  
      { id: 'y7', cat: 'yam', name: 'ยำเส้นแก้วหมูสับ', price: 79, emoji: '🥗' },  
      { id: 'y8', cat: 'yam', name: 'ยำเส้นแก้วทะเล', price: 99, emoji: '🦑' },  
      { id: 'y9', cat: 'yam', name: 'ยำคะน้ากุ้งสด', price: 99, emoji: '🦐' },  
      { id: 'y10', cat: 'yam', name: 'พล่ากุ้งเห็ดเข็มทอง', price: 109, emoji: '🍄' },  
      { id: 'y11', cat: 'yam', name: 'ยำรวมทะเล', price: 119, emoji: '🦪' },  
      { id: 'y12', cat: 'yam', name: 'ยำผักบุ้งกรอบ', price: 129, emoji: '🌿' },  
      { id: 'y13', cat: 'yam', name: 'ยำเห็ดเข็มทองกรอบ', price: 129, emoji: '🍄' },  
      { id: 'y14', cat: 'yam', name: 'ยำหอยนางรม', price: 129, emoji: '🦪' },  
      // Fried (เมนูทอด)  
      { id: 'f1', cat: 'fried', name: 'ถั่วทอดสมุนไพร', price: 39, emoji: '🥜' },  
      { id: 'f2', cat: 'fried', name: 'เม็ดมะม่วงหิมพานต์ทอด', price: 59, emoji: '🥜' },  
      { id: 'f3', cat: 'fried', name: 'เฟรนฟราย', price: 69, emoji: '🍟' },  
      { id: 'f4', cat: 'fried', name: 'ไก่จ๊อทอด', price: 69, emoji: '🍢' },  
      { id: 'f5', cat: 'fried', name: 'นักเก็ตไก่', price: 69, emoji: '🍗' },  
      { id: 'f6', cat: 'fried', name: 'เกี๊ยวซ่า', price: 69, emoji: '🥟' },  
      { id: 'f7', cat: 'fried', name: 'ปอเปี๊ยะทอด', price: 69, emoji: '🌯' },  
      { id: 'f8', cat: 'fried', name: 'ถุงทองทอดกรอบ', price: 69, emoji: '🥟' },  
      { id: 'f9', cat: 'fried', name: 'ปีกไก่ทอด', price: 79, emoji: '🍗' },  
      { id: 'f10', cat: 'fried', name: 'เอ็นไก่ทอด', price: 79, emoji: '🍗' },  
      { id: 'f11', cat: 'fried', name: 'ไก่ทอดซอสเกาหลี', price: 89, emoji: '🍗' },  
      { id: 'f12', cat: 'fried', name: 'ไก่ทอดซอสครีมมะนาว', price: 99, emoji: '🍋' },  
      { id: 'f13', cat: 'fried', name: 'หมูสามชั้นทอดน้ำปลา', price: 119, emoji: '🥓' },  
      { id: 'f14', cat: 'fried', name: 'แหนมซี่โครงทอด', price: 129, emoji: '🍖' },  
      { id: 'f15', cat: 'fried', name: 'ปลานิลทอดน้ำปลา', price: 169, emoji: '🐟' },  
      { id: 'f16', cat: 'fried', name: 'ฉู่ฉี่ปลานิล', price: 179, emoji: '🐟' },  
      { id: 'f17', cat: 'fried', name: 'ปลานิลคั่วพริกเกลือ', price: 189, emoji: '🐟' },  
      { id: 'f18', cat: 'fried', name: 'ปลานิลยำสมุนไพร', price: 199, emoji: '🐟' },  
      { id: 'f19', cat: 'fried', name: 'ปลานิลทอดสมุนไพร', price: 199, emoji: '🐟' },  
      { id: 'f20', cat: 'fried', name: 'ปลานิลมันตึงลำ (Signature)', price: 209, emoji: '👑' },  
      // Soup (ต้ม / แกง)  
      { id: 'sp1', cat: 'soup', name: 'แกงจืดไข่น้ำ', price: 89, emoji: '🍲' },  
      { id: 'sp2', cat: 'soup', name: 'แกงจืดเต้าหู้หมูสับ', price: 99, emoji: '🍲' },  
      { id: 'sp3', cat: 'soup', name: 'ต้มแซ่บเอ็นไก่', price: 109, emoji: '🥣' },  
      { id: 'sp4', cat: 'soup', name: 'ต้มแซ่บกระดูกอ่อน', price: 139, emoji: '🍖' },  
      { id: 'sp5', cat: 'soup', name: 'ต้มแซ่บปลานิล', price: 139, emoji: '🐟' },  
      { id: 'sp6', cat: 'soup', name: 'ต้มยำปลาดอลลี่', price: 139, emoji: '🍲' },  
      { id: 'sp7', cat: 'soup', name: 'ต้มยำปลานิล', price: 159, emoji: '🐟' },  
      { id: 'sp8', cat: 'soup', name: 'ต้มยำทะเล', price: 169, emoji: '🦐' },  
      { id: 'sp9', cat: 'soup', name: 'โป๊ะแตกทะเล', price: 169, emoji: '🦑' },  
      { id: 'sp10', cat: 'soup', name: 'แกงส้มปลานิลทอด', price: 209, emoji: '🍲' },  
      // Appetizer (กับแกล้ม)  
      { id: 'ap1', cat: 'appetizer', name: 'ปูอัดวาซาบิ', price: 69, emoji: '🦀' },  
      { id: 'ap2', cat: 'appetizer', name: 'ไข่เยี่ยวม้าทรงเครื่อง', price: 79, emoji: '🥚' },  
      { id: 'ap3', cat: 'appetizer', name: 'แหนมทรงเครื่อง', price: 89, emoji: '🥗' },  
      { id: 'ap4', cat: 'appetizer', name: 'กุ้งแช่น้ำปลา', price: 89, emoji: '🦐' },  
      { id: 'ap5', cat: 'appetizer', name: 'หมูมะนาว', price: 89, emoji: '🥩' },  
      { id: 'ap6', cat: 'appetizer', name: 'ปลาลวกจิ้ม', price: 99, emoji: '🐟' },  
      { id: 'ap7', cat: 'appetizer', name: 'ทะเลลวกจิ้ม', price: 119, emoji: '🦑' },  
      // Stirfry (เมนูผัด)  
      { id: 'sf1', cat: 'stirfry', name: 'กะหล่ำผัดน้ำปลา', price: 69, emoji: '🥬' },  
      { id: 'sf2', cat: 'stirfry', name: 'ผักบุ้งไฟแดง', price: 69, emoji: '🌿' },  
      { id: 'sf3', cat: 'stirfry', name: 'ผักบุ้งไฟแดงหมูสับ', price: 89, emoji: '🌿' },  
      { id: 'sf4', cat: 'stirfry', name: 'คะน้าน้ำมันหอย', price: 69, emoji: '🥦' },  
      { id: 'sf5', cat: 'stirfry', name: 'คะน้าน้ำมันหอยหมู', price: 89, emoji: '🥦' },  
      { id: 'sf6', cat: 'stirfry', name: 'คะน้าน้ำมันหอยทะเล', price: 109, emoji: '🦐' },  
      { id: 'sf7', cat: 'stirfry', name: 'ผัดผักรวม', price: 69, emoji: '🥗' },  
      { id: 'sf8', cat: 'stirfry', name: 'ผัดผักรวมหมู', price: 89, emoji: '🥗' },  
      { id: 'sf9', cat: 'stirfry', name: 'ผัดผักรวมทะเล', price: 109, emoji: '🦐' },  
      { id: 'sf10', cat: 'stirfry', name: 'ผัดกะเพราไข่เยี่ยวม้า', price: 109, emoji: '🌿' },  
      { id: 'sf11', cat: 'stirfry', name: 'ผัดผงกะหรี่ทะเล', price: 139, emoji: '🍛' },  
      { id: 'sf12', cat: 'stirfry', name: 'ผัดฉ่าทะเล', price: 139, emoji: '🌶️' },  
      { id: 'sf13', cat: 'stirfry', name: 'ข้าวผัดหมู (จานใหญ่)', price: 129, emoji: '🍚' },  
      { id: 'sf14', cat: 'stirfry', name: 'ข้าวผัดทะเล (จานใหญ่)', price: 159, emoji: '🦐' },  
      // Single dish (อาหารจานเดียว)  
      { id: 'sd1', cat: 'singledish', name: 'ข้าวไข่เจียวหมูสับ', price: 50, emoji: '🍳' },  
      { id: 'sd2', cat: 'singledish', name: 'ข้าวผัดหมู', price: 55, emoji: '🍚' },  
      { id: 'sd3', cat: 'singledish', name: 'ข้าวผัดทะเล', price: 65, emoji: '🦐' },  
      { id: 'sd4', cat: 'singledish', name: 'ข้าวหมูกระเทียม', price: 55, emoji: '🧄' },  
      { id: 'sd5', cat: 'singledish', name: 'ข้าวกะเพราหมู', price: 55, emoji: '🌿' },  
      { id: 'sd6', cat: 'singledish', name: 'ข้าวกะเพราทะเล', price: 65, emoji: '🦐' },  
      { id: 'sd7', cat: 'singledish', name: 'ข้าวผัดพริกสดหมู', price: 55, emoji: '🌶️' },  
      { id: 'sd8', cat: 'singledish', name: 'ข้าวผัดพริกสดทะเล', price: 65, emoji: '🌶️' },  
      { id: 'sd9', cat: 'singledish', name: 'ข้าวผัดพริกแกงหมู', price: 55, emoji: '🍛' },  
      { id: 'sd10', cat: 'singledish', name: 'ข้าวผัดพริกแกงทะเล', price: 65, emoji: '🍛' },  
      { id: 'sd11', cat: 'singledish', name: 'ข้าวผัดคะน้าหมู', price: 55, emoji: '🥬' },  
      { id: 'sd12', cat: 'singledish', name: 'ข้าวผัดคะน้าทะเล', price: 65, emoji: '🥬' },  
      { id: 'sd13', cat: 'singledish', name: 'สุกี้หมู', price: 55, emoji: '🍜' },  
      { id: 'sd14', cat: 'singledish', name: 'สุกี้ทะเล', price: 65, emoji: '🦐' },  
      { id: 'sd15', cat: 'singledish', name: 'สปาเก็ตตี้ผัดขี้เมาหมู', price: 79, emoji: '🍝' },  
      { id: 'sd16', cat: 'singledish', name: 'สปาเก็ตตี้ผัดขี้เมาทะเล', price: 89, emoji: '🍝' },  
      { id: 'sd17', cat: 'singledish', name: 'ไข่ดาว', price: 10, emoji: '🍳' },  
      { id: 'sd18', cat: 'singledish', name: 'ไข่เจียว', price: 59, emoji: '🍳' },  
      { id: 'sd19', cat: 'singledish', name: 'ไข่เจียวหมูสับ', price: 79, emoji: '🍳' },  
      { id: 'sd20', cat: 'singledish', name: 'ข้าวสวย', price: 15, emoji: '🍚' }  
    ];  
  
    const TABLES = ['โต๊ะ 1', 'โต๊ะ 2', 'โต๊ะ 3', 'โต๊ะ 4', 'โต๊ะ 5', 'โต๊ะ 6', 'โต๊ะ 7', 'โต๊ะ 8', 'โซฟา 1', 'โซฟา 2', 'สนาม 1', 'สนาม 2'];  
  
    function showToast(msg) {  
      const b = document.getElementById('toastBanner');  
      b.textContent = msg;  
      b.classList.remove('hidden');  
      setTimeout(() => b.classList.add('hidden'), 2500);  
    }  
  
    function renderCategories() {  
      const container = document.getElementById('categoryTabsContainer');  
      container.innerHTML = '';  
      CATEGORIES.forEach(cat => {  
        const isActive = state.activeCategory === cat.id;  
        const btn = document.createElement('button');  
        btn.type = 'button';  
        btn.className = `shrink-0 px-3.5 py-2 rounded-2xl text-xs sm:text-sm font-semibold flex items-center gap-1.5 transition ${  
          isActive  
            ? 'bg-amber-500 text-stone-950 shadow-md scale-[1.02]'  
            : 'bg-stone-900 text-stone-300 border border-stone-800 hover:border-amber-500/40'  
        }`;  
        btn.innerHTML = `<span>${cat.icon}</span><span>${cat.name}</span>`;  
        btn.addEventListener('click', () => {  
          state.activeCategory = cat.id;  
          renderCategories();  
          renderMenu();  
        });  
        container.appendChild(btn);  
      });  
    }  
  
    function renderMenu() {  
      const grid = document.getElementById('menuGridContainer');  
      grid.innerHTML = '';  
      const filtered = MENU_ITEMS.filter(i => state.activeCategory === 'all' || i.cat === state.activeCategory);  
      document.getElementById('filteredItemCount').textContent = `${filtered.length} รายการ`;  
  
      filtered.forEach(item => {  
        const div = document.createElement('div');  
        div.className = 'p-3.5 rounded-2xl bg-stone-900 border border-stone-800 flex items-center justify-between gap-3';  
        div.innerHTML = `  
          <div class="flex items-center gap-3">  
            <div class="w-12 h-12 rounded-xl bg-stone-800 flex items-center justify-center text-2xl">${item.emoji}</div>  
            <div>  
              <div class="font-bold text-sm text-white">${item.name}</div>  
              <div class="text-xs text-amber-400 font-extrabold mt-0.5">${item.price} ฿</div>  
            </div>  
          </div>  
          <button type="button" class="px-3 py-2 rounded-xl bg-amber-500 hover:bg-amber-400 text-stone-950 font-bold text-xs shrink-0">+ เลือก</button>  
        `;  
        div.querySelector('button').addEventListener('click', () => {  
          const ex = state.cart.find(c => c.id === item.id);  
          if (ex) { ex.qty += 1; } else { state.cart.push({ ...item, qty: 1 }); }  
          updateCartUI();  
          showToast(`เพิ่ม "${item.name}" แล้ว`);  
        });  
        grid.appendChild(div);  
      });  
    }  
  
    function updateCartUI() {  
      const totalQty = state.cart.reduce((s, i) => s + i.qty, 0);  
      const totalPrice = state.cart.reduce((s, i) => s + i.qty * i.price, 0);  
      document.getElementById('cartBadgeCount').textContent = totalQty;  
      document.getElementById('cartBarTotalPrice').textContent = `${totalPrice.toLocaleString()} ฿`;  
      document.getElementById('cartBarTableText').textContent = state.currentTable;  
      document.getElementById('cartModalTableName').textContent = state.currentTable;  
      document.getElementById('cartModalTotalPrice').textContent = `${totalPrice.toLocaleString()} ฿`;  
  
      const list = document.getElementById('cartItemsList');  
      list.innerHTML = '';  
      if (state.cart.length === 0) {  
        list.innerHTML = `<div class="py-8 text-center text-xs text-stone-500">ตะกร้าว่างเปล่า</div>`;  
        return;  
      }  
      state.cart.forEach((c, idx) => {  
        const row = document.createElement('div');  
        row.className = 'p-3 rounded-xl bg-stone-950 border border-stone-800 flex items-center justify-between text-xs';  
        row.innerHTML = `  
          <div><span class="font-bold text-white">${c.name}</span><div class="text-amber-400">${c.price} ฿</div></div>  
          <div class="flex items-center gap-2">  
            <button class="w-7 h-7 bg-stone-800 text-white rounded font-bold" onclick="changeQty(${idx}, -1)">-</button>  
            <span class="font-bold text-amber-400">${c.qty}</span>  
            <button class="w-7 h-7 bg-stone-800 text-white rounded font-bold" onclick="changeQty(${idx}, 1)">+</button>  
          </div>  
        `;  
        list.appendChild(row);  
      });  
    }  
  
    window.changeQty = function(idx, delta) {  
      state.cart[idx].qty += delta;  
      if (state.cart[idx].qty <= 0) state.cart.splice(idx, 1);  
      updateCartUI();  
    };  
  
    function sendToLine() {  
      if (state.cart.length === 0) {  
        showToast('กรุณาเลือกเมนูก่อนครับ');  
        return;  
      }  
      let text = `🚨 สั่งอาหารจาก ${state.currentTable}\n------------------\n`;  
      let total = 0;  
      state.cart.forEach(i => {  
        text += `• ${i.name} x${i.qty} (${i.price * i.qty}฿)\n`;  
        total += i.price * i.qty;  
      });  
      text += `------------------\n💰 ยอดรวม: ${total} บาท`;  
  
      const encoded = encodeURIComponent(text);  
      const lineUrl = `https://line.me/R/oaMessage/${LINE_OA_ID}/?${encoded}`;[span_2](start_span)[span_2](end_span)  
      window.location.href = lineUrl;  
    }  
  
    function init() {  
      renderCategories();  
      renderMenu();  
      updateCartUI();  
  
      document.getElementById('btnOpenCartDrawer').addEventListener('click', () => {  
        document.getElementById('cartDrawerModal').classList.remove('hidden');  
      });  
      document.getElementById('btnCloseCartModal').addEventListener('click', () => {  
        document.getElementById('cartDrawerModal').classList.add('hidden');  
      });  
      document.getElementById('btnSubmitToLine').addEventListener('click', sendToLine);  
  
      document.getElementById('btnOpenTableModal').addEventListener('click', () => {  
        const grid = document.getElementById('tableOptionsGrid');  
        grid.innerHTML = '';  
        TABLES.forEach(t => {  
          const b = document.createElement('button');  
          b.className = 'p-2 rounded-xl bg-stone-800 text-xs font-bold text-white border border-stone-700';  
          b.textContent = t;  
          b.addEventListener('click', () => {  
            state.currentTable = t;  
            document.getElementById('currentTableLabel').textContent = t;  
            document.getElementById('tableSelectModal').classList.add('hidden');  
            showToast(`เปลี่ยนเป็น ${t} แล้ว`);  
            updateCartUI();  
          });  
          grid.appendChild(b);  
        });  
        document.getElementById('tableSelectModal').classList.remove('hidden');  
      });  
      document.getElementById('btnCloseTableModal').addEventListener('click', () => {  
        document.getElementById('tableSelectModal').classList.add('hidden');  
      });  
    }  
  
    window.addEventListener('DOMContentLoaded', init);  
  </script>  
</body>  
</html>  
