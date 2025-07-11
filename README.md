<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no" />
<title>BBG Metin RPG - Gelişmiş Versiyon</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Orbitron:wght@500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
<style>
  :root {
    --primary-accent: #00e68a; /* Neon Green */
    --secondary-accent: #a020f0; /* Vibrant Purple */
    --background-dark: #1a1a2e; /* Deep Blue-Purple */
    --background-medium: #212a49; /* Slightly lighter */
    --background-light: #2c3a62; /* Even lighter for content */
    --text-color: #e0e0e0;
    --border-color: #3f4a70;
    --button-bg: var(--primary-accent);
    --button-text: #1a1a2e;
    --button-hover-bg: #00ff99;
    --red-alert: #ff4d4d; /* Kırmızı hala gerekli olabilir, örneğin uyarılar için */
    --blue-info: #3399ff;
    --gold-color: #ffeb3b;
    --neon-green-heart: #00e68a; /* Neon yeşili kalp rengi */

    --border-radius-sm: 6px;
    --border-radius-md: 10px;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; user-select: none; }
  body {
    font-family: 'Inter', sans-serif; /* Modern, readable font */
    background: linear-gradient(135deg, #120a2e 0%, #0a051a 100%); /* Darker, more mysterious gradient */
    color: var(--text-color);
    overflow: hidden;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  /* Subtle background pattern */
  body::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: radial-gradient(circle, #3a2e5a 1px, transparent 1px);
    background-size: 20px 20px;
    opacity: 0.1;
    z-index: -1;
  }

  #gameWrapper {
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 420px;
    height: 100%;
    max-height: 780px; /* Slightly taller */
    background: var(--background-dark);
    border-radius: var(--border-radius-md);
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.6), 0 0 0 2px var(--border-color); /* Stronger shadow and border */
    overflow: hidden;
    position: relative;
  }

  #topBar {
    background: var(--background-dark);
    padding: 10px 15px;
    font-size: 11px;
    display: grid; /* Use grid for better alignment */
    grid-template-columns: 1fr 1fr; /* Two columns */
    gap: 5px 0; /* Vertical gap between rows */
    border-bottom: 1px solid var(--border-color);
    position: relative;
    z-index: 10;
  }
  #topBar .stat-item { /* Class for each stat div */
    display: flex;
    align-items: center;
    font-family: 'Orbitron', sans-serif; /* Digital font for stats */
    font-weight: 500;
  }
  #topBar .icon {
    font-size: 15px;
    margin-right: 6px;
    color: var(--primary-accent);
    text-shadow: 0 0 5px rgba(0,255,170,0.5); /* Neon glow */
  }
  /* Neon green kalpler */
  #topBar .stat-item i.fa-heart {
    color: var(--neon-green-heart);
    text-shadow: 0 0 5px rgba(0,255,170,0.5);
  }

  #expBarContainer {
    background: var(--background-medium);
    width: 100%;
    height: 10px; /* Thicker bar */
    border-bottom: 1px solid var(--border-color);
    position: relative;
    overflow: hidden;
  }
  #expBar {
    height: 100%;
    background: linear-gradient(90deg, var(--primary-accent), #00ffcc); /* Gradient fill */
    width: 0%;
    transition: width 0.4s cubic-bezier(0.25, 0.8, 0.25, 1); /* Smoother animation */
    box-shadow: 0 0 8px var(--primary-accent); /* Glow effect */
  }
  #expBar::after {
    content: '';
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    width: 20px;
    background: rgba(255,255,255,0.2);
    transform: skewX(-20deg);
    animation: shine 2s infinite; /* Subtle shine animation */
  }
  @keyframes shine {
    0% { left: -20%; }
    100% { left: 100%; }
  }

  #taskBar {
    background: var(--background-medium);
    padding: 10px 15px;
    font-size: 11px;
    text-align: center;
    border-bottom: 1px solid var(--border-color);
    color: var(--gold-color); /* Gold color for tasks */
    min-height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 600;
    text-shadow: 0 0 4px rgba(255,235,59,0.3);
  }

  #screenContent {
    flex-grow: 1;
    overflow-y: auto;
    padding: 18px;
    background: var(--background-light);
    font-size: 13px;
    position: relative;
    transition: opacity 0.3s ease-in-out; /* Smooth transition for screen changes */
  }
  /* Mining screen specific styles */
  #screenContent.mine-screen {
    background: var(--background-light); /* Base color */
    background-image:
        linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.3)), /* Darker overlay */
        url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjAiIGhlaWdodD0iMjAiIHZpZXdCb3g9IjAgMCAyMCAyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48Y2lyY2xlIGN4PSIxMCIgY3k9IjEwIiByPSIyIiBmaWxsPSIjNGE1ODg3IiBvcGFjaXR5PSIwLjMiLz48L3N2Z3I+'); /* Subtle dots */
    background-size: 40px 40px;
    animation: backgroundPan 30s linear infinite; /* Slow pan animation */
  }

  @keyframes backgroundPan {
    0% { background-position: 0% 0%; }
    100% { background-position: 100% 100%; }
  }


  #bottomBar {
    display: flex;
    justify-content: space-around;
    background: var(--background-dark);
    border-top: 1px solid var(--border-color);
    user-select: none;
    padding: 5px 0;
    box-shadow: 0 -5px 20px rgba(0,0,0,0.4);
    position: relative;
    z-index: 10;
  }
  .bottomBtn {
    flex: 1;
    padding: 10px 0;
    font-size: 11px;
    text-align: center;
    border-right: 1px solid rgba(255,255,255,0.05); /* Subtle separator */
    cursor: pointer;
    user-select: none;
    color: var(--text-color);
    transition: background-color 0.2s ease, color 0.2s ease, transform 0.1s ease;
    position: relative;
    overflow: hidden;
  }
  .bottomBtn:last-child {
    border-right: none;
  }
  .bottomBtn:hover {
    background-color: var(--background-medium);
    color: var(--primary-accent);
    transform: translateY(-2px);
  }
  .bottomBtn span {
    display: block;
    font-size: 22px;
    margin-bottom: 5px;
    color: var(--secondary-accent); /* Icon color */
    transition: color 0.2s ease, transform 0.1s ease;
  }
  .bottomBtn:hover span {
    color: var(--primary-accent);
    transform: scale(1.1);
  }

  button {
    background: var(--button-bg);
    color: var(--button-text);
    border: none;
    padding: 10px 18px;
    border-radius: var(--border-radius-sm);
    font-size: 12px;
    margin: 8px 0;
    cursor: pointer;
    user-select: none;
    font-family: 'Inter', sans-serif;
    font-weight: 700;
    transition: background-color 0.2s ease, transform 0.1s ease, box-shadow 0.2s ease;
    text-transform: uppercase;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
    letter-spacing: 0.5px;
  }
  button:hover:not(:disabled) {
    background: var(--button-hover-bg);
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
  }
  button:disabled {
    background: #555;
    cursor: not-allowed;
    opacity: 0.6;
    box-shadow: none;
  }

  h2 { font-family: 'Orbitron', sans-serif; font-size: 18px; margin: 15px 0; color: var(--primary-accent); text-align: center; text-shadow: 0 0 8px rgba(0,255,170,0.6); }
  h3 { font-family: 'Orbitron', sans-serif; font-size: 14px; margin: 12px 0 8px; color: var(--text-color); border-bottom: 1px dashed var(--border-color); padding-bottom: 6px; }

  .item-row {
    display: flex;
    justify-content: space-between;
    padding: 10px 0;
    border-bottom: 1px solid var(--border-color);
    align-items: center;
    flex-wrap: wrap; /* Allow wrapping for buttons */
  }
  .item-row:last-child {
      border-bottom: none;
  }
  .item-row div {
      flex: 1;
      text-align: left;
      display: flex;
      align-items: center;
  }
  .item-row div:nth-child(2) {
      flex: 0 0 auto;
      text-align: center;
      min-width: 70px;
      font-family: 'Orbitron', sans-serif;
  }
  .item-row .item-actions { /* New class for button container */
      flex: 0 0 auto;
      text-align: right;
      display: flex;
      gap: 5px; /* Space between buttons */
      margin-top: 5px; /* Give space if wrapped */
      width: 100%; /* Take full width on wrap */
      justify-content: flex-end; /* Align to right */
  }
  @media (min-width: 380px) { /* Adjust for larger screens to keep buttons on one line */
    .item-row .item-actions {
      width: auto;
      margin-top: 0;
    }
  }

  .item-row .fa-solid {
      font-size: 18px;
      margin-right: 8px;
      color: var(--secondary-accent);
  }

  .mine-icon {
    position: absolute;
    font-size: 28px; /* Larger icon */
    color: var(--gold-color); /* Gold for item drops */
    pointer-events: none;
    user-select: none;
    animation: minePop 0.9s cubic-bezier(0.17, 0.84, 0.44, 1) forwards; /* Bouncier animation */
    font-weight: bold;
    text-shadow: 0 0 8px rgba(255,255,0,0.7); /* Stronger glow */
  }
  @keyframes minePop {
    0% { opacity: 1; transform: translateY(0) scale(1); }
    50% { transform: translateY(-30px) scale(1.3); } /* Pop up more */
    100% { opacity: 0; transform: translateY(-70px) scale(1.6); } /* Fade out higher */
  }

  /* Centered and fading item drop text */
  .item-drop-text, .coin-drop-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 24px; /* Larger text */
    font-weight: bold;
    color: var(--gold-color); /* Gold color */
    text-shadow: 0 0 10px rgba(255,235,59,0.8);
    opacity: 0;
    animation: fadeInOutUp 1.5s ease-out forwards;
    pointer-events: none;
    white-space: nowrap;
    z-index: 5000; /* Ensure it's above other content */
  }

  @keyframes fadeInOutUp {
    0% { opacity: 0; transform: translate(-50%, 0px); }
    20% { opacity: 1; transform: translate(-50%, -10px); }
    80% { opacity: 1; transform: translate(-50%, -40px); }
    100% { opacity: 0; transform: translate(-50%, -60px); }
  }


  .craft-item {
    margin-bottom: 10px;
    padding: 12px;
    background: var(--background-medium);
    border-radius: var(--border-radius-sm);
    border: 1px solid var(--border-color);
    box-shadow: 0 2px 8px rgba(0,0,0,0.2);
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
  .craft-item .fa-solid {
      font-size: 18px;
      margin-right: 8px;
      color: var(--secondary-accent);
  }
  .craft-item strong {
      font-size: 14px;
      color: var(--primary-accent);
      margin-bottom: 8px;
      display: flex;
      align-items: center;
  }
  .craft-item div {
      font-size: 12px;
      margin-bottom: 8px;
  }
  .craft-item button {
    align-self: flex-end; /* Button to the right */
    margin-top: 10px;
  }

  .task-item {
    margin: 10px 0;
    padding: 12px;
    background: var(--background-medium);
    border-radius: var(--border-radius-sm);
    border: 1px solid var(--border-color);
    box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  }
  .task-item.completed {
    background: #004d00; /* Darker green for completed */
    color: #aaffaa;
    text-decoration: line-through;
    opacity: 0.7;
  }
  .task-reward {
    color: var(--primary-accent);
    font-weight: bold;
    margin-top: 8px;
    font-family: 'Orbitron', sans-serif;
  }

  input[type=number], input[type=text] {
    width: 100%;
    padding: 10px 12px;
    margin: 8px 0;
    background: var(--background-medium);
    border: 1px solid var(--border-color);
    color: var(--text-color);
    border-radius: var(--border-radius-sm);
    font-family: 'Inter', sans-serif;
    font-size: 13px;
    box-shadow: inset 0 1px 3px rgba(0,0,0,0.3);
  }
  input[type=number]:focus, input[type=text]:focus {
    outline: none;
    border-color: var(--primary-accent);
    box-shadow: 0 0 0 2px rgba(0,255,170,0.3);
  }

  .sell-button, .use-button, .repair-button, .purchase-button {
    background: var(--red-alert);
    color: var(--button-text); /* Ensure text color is readable */
    border: none;
    padding: 8px 12px;
    border-radius: var(--border-radius-sm);
    cursor: pointer;
    font-size: 11px;
    user-select: none;
    box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    font-weight: 600;
    transition: background-color 0.2s ease, transform 0.1s ease, box-shadow 0.2s ease;
  }
  .use-button {
    background: var(--blue-info);
  }
  .repair-button {
    background: var(--secondary-accent); /* Purple for repair */
  }
  .purchase-button { /* New button style for purchase */
    background: var(--primary-accent); /* Green for buy */
    color: var(--button-text);
  }
  .sell-button:hover:not(:disabled), .use-button:hover:not(:disabled), .repair-button:hover:not(:disabled), .purchase-button:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.3);
  }
  .sell-button:disabled, .use-button:disabled, .repair-button:disabled, .purchase-button:disabled {
    background: #777;
    cursor: not-allowed;
    opacity: 0.6;
    box-shadow: none;
  }

  /* Scrollbar */
  #screenContent::-webkit-scrollbar {
    width: 8px;
  }
  #screenContent::-webkit-scrollbar-track {
    background: var(--background-medium);
  }
  #screenContent::-webkit-scrollbar-thumb {
    background: var(--primary-accent);
    border-radius: 4px;
  }
  #screenContent::-webkit-scrollbar-thumb:hover {
    background: var(--button-hover-bg);
  }

  /* Notice styling */
  .notice {
    position: fixed;
    top: 25px;
    left: 50%;
    transform: translateX(-50%);
    background: linear-gradient(45deg, var(--primary-accent), #00ffcc);
    color: var(--background-dark);
    padding: 12px 25px;
    border-radius: var(--border-radius-md);
    font-size: 15px;
    font-weight: bold;
    z-index: 10000;
    box-shadow: 0 6px 20px rgba(0,0,0,0.4);
    animation: fadeOut 3.5s forwards;
    opacity: 1;
    font-family: 'Inter', sans-serif;
    white-space: nowrap; /* Prevent text wrapping */
  }

  @keyframes fadeOut {
    0% { opacity: 1; transform: translateX(-50%) translateY(0); }
    80% { opacity: 1; transform: translateX(-50%) translateY(0); }
    100% { opacity: 0; transform: translateX(-50%) translateY(-30px); }
  }

  /* Sale confirmation popup (Now generalized for any small message) */
  .small-popup {
    position: fixed;
    bottom: 80px; /* Above the bottom bar */
    left: 50%;
    transform: translateX(-50%);
    background: linear-gradient(90deg, var(--gold-color), #ffd700);
    color: var(--background-dark);
    padding: 10px 20px;
    border-radius: var(--border-radius-sm);
    font-size: 14px;
    font-weight: bold;
    z-index: 9999;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    white-space: nowrap;
    opacity: 0;
    animation: slideInFadeOut 2.5s forwards;
  }

  @keyframes slideInFadeOut {
    0% { opacity: 0; transform: translate(-50%, 20px); }
    10% { opacity: 1; transform: translate(-50%, 0); }
    80% { opacity: 1; transform: translate(-50%, 0); }
    100% { opacity: 0; transform: translate(-50%, -20px); }
  }


  /* Bank Button (Top Right) */
  #btnBankTop {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 40px; /* Small square button */
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: var(--secondary-accent);
    color: white;
    border-radius: var(--border-radius-md);
    box-shadow: 0 4px 15px rgba(0,0,0,0.4);
    cursor: pointer;
    z-index: 100; /* Ensure it's above other elements */
    transition: background-color 0.2s ease, transform 0.1s ease;
  }
  #btnBankTop:hover {
    background: #c040f0; /* Lighter purple on hover */
    transform: translateY(-2px);
  }
  #btnBankTop i {
    font-size: 20px;
  }

  /* Admin Button (Below Bank Button) */
  #btnAdminTop {
    position: absolute;
    top: 60px; /* Adjust this value to position below bank button */
    right: 10px;
    width: 40px;
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #ff7700; /* Orange for admin */
    color: white;
    border-radius: var(--border-radius-md);
    box-shadow: 0 4px 15px rgba(0,0,0,0.4);
    cursor: pointer;
    z-index: 100;
    transition: background-color 0.2s ease, transform 0.1s ease;
    display: none; /* Hidden by default */
  }
  #btnAdminTop:hover {
    background: #ff9933; /* Lighter orange on hover */
    transform: translateY(-2px);
  }
  #btnAdminTop i {
    font-size: 20px;
  }

  /* Modal Styles (Bank, Sale, Purchase, Market, Admin) */
  #bankModal, #saleDetailModal, #marketModal, #purchaseDetailModal, #adminModal {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.8); /* Dark overlay */
    display: none; /* Hidden by default */
    justify-content: center;
    align-items: center;
    z-index: 1000; /* Above all other game content */
    padding: 10px; /* Add some padding for smaller screens */
  }
  #bankModal.active, #saleDetailModal.active, #marketModal.active, #purchaseDetailModal.active, #adminModal.active {
    display: flex;
  }
  #bankContent, #saleDetailModalContent, #marketContent, #purchaseDetailModalContent, #adminContent { /* Combined styling */
    background: var(--background-dark);
    padding: 20px; /* Slightly reduced padding */
    border-radius: var(--border-radius-md);
    width: 90%;
    max-width: 350px; /* Make it smaller */
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.6), 0 0 0 2px var(--primary-accent); /* Neon border */
    position: relative;
    transform: scale(0.9);
    opacity: 0;
    animation: modalOpen 0.3s forwards cubic-bezier(0.2, 0.8, 0.2, 1);
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow-y: auto; /* Allow scroll if content is too long */
    max-height: 90vh; /* Prevent modal from exceeding screen height */
  }
  @keyframes modalOpen {
    from { transform: scale(0.9); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
  }
  #bankContent .close-button, #saleDetailModal .close-button, #marketModal .close-button, #purchaseDetailModal .close-button, #adminModal .close-button {
    position: absolute;
    top: 5px; /* Adjust close button position */
    right: 5px;
    background: none;
    border: none;
    color: var(--text-color);
    font-size: 28px; /* Make close button larger */
    cursor: pointer;
    padding: 5px;
  }
  #bankContent .close-button:hover, #saleDetailModal .close-button:hover, #marketModal .close-button:hover, #purchaseDetailModal .close-button:hover, #adminModal .close-button:hover {
    color: var(--red-alert);
  }
  #bankContent h2, #saleDetailModal h2, #marketModal h2, #purchaseDetailModal h2, #adminModal h2 {
    margin-top: 5px; /* Adjust heading margin */
    margin-bottom: 10px;
  }
  #bankContent h3, #saleDetailModal h3, #marketModal h3, #purchaseDetailModal h3, #adminModal h3 {
    margin-top: 15px;
  }

  .market-item {
    display: flex;
    justify-content: space-between;
    align-items: center; /* Dikey hizalama */
    width: 100%;
    padding: 10px 0;
    border-bottom: 1px dashed var(--border-color);
    font-size: 14px;
    font-family: 'Orbitron', sans-serif;
  }
  .market-item:last-child {
    border-bottom: none;
  }
  .market-item .item-name {
    display: flex;
    align-items: center;
    font-weight: 600;
    color: var(--text-color);
    flex: 1; /* Allow name to take available space */
    text-align: left;
  }
  .market-item .item-price-group {
    display: flex;
    align-items: center; /* Yan yana hizalama için */
    font-weight: 700;
    gap: 5px; /* Ok ile fiyat arasına boşluk */
    flex: 0 0 auto; /* Content'i sıkışmasın */
  }
  .market-item .item-price {
    font-size: 1.2em;
  }
  .market-item .price-arrow { /* Ok için yeni sınıf */
    font-size: 0.9em;
  }
  .market-item .price-up {
    color: #00ff00; /* Green for up */
  }
  .market-item .price-down {
    color: var(--red-alert); /* Red for down */
  }

  /* Responsive Adjustments */
  @media (max-width: 360px) {
    #topBar div {
      font-size: 10px;
    }
    #topBar .icon {
      font-size: 13px;
    }
    .bottomBtn {
      font-size: 10px;
      padding: 8px 0;
    }
    .bottomBtn span {
      font-size: 20px;
    }
    button {
      padding: 8px 12px;
      font-size: 11px;
    }
    h2 { font-size: 16px; }
    h3 { font-size: 13px; }
    .item-row .fa-solid { font-size: 16px; }
    .mine-icon { font-size: 24px; }
    #btnBankTop, #btnAdminTop {
      width: 35px;
      height: 35px;
      font-size: 18px;
    }
    #bankContent, #saleDetailModalContent, #marketContent, #purchaseDetailModalContent, #adminContent {
        padding: 15px;
    }
  }
</style>
</head>
<body>
<div id="gameWrapper">
  <div id="topBar">
    <div class="stat-item"><i class="fa-solid fa-money-bill-wave icon"></i> Bakiye: <span id="tlValue">0.00 TL</span></div>
    <div class="stat-item"><i class="fa-solid fa-hammer icon"></i> <span id="pickaxeType">Taş Kazma</span> <span id="currentPickaxeDurability"><i class="fa-solid fa-heart"></i>100</span></div>
    <div class="stat-item"><i class="fa-solid fa-battery-full icon"></i> Enerji: <span id="energy">100</span></div>
    <div class="stat-item"><i class="fa-solid fa-star icon"></i> Seviye: <span id="level">1</span></div>

    <div class="stat-item" style="grid-column: 1 / 2; justify-content: flex-start; font-size: 10px; color: #aaa;">
      <i class="fa-brands fa-telegram icon" style="font-size: 14px;"></i> <span id="telegramUserInfo">@KullaniciAdi (ID: 123456)</span>
    </div>
    <div class="stat-item" style="grid-column: 2 / 3; justify-content: flex-start;"><i class="fa-solid fa-coins icon" style="color: var(--primary-accent); text-shadow: 0 0 5px rgba(0,255,170,0.5);"></i> Coin: <span id="coinValue">0</span></div>
  </div>
  <div id="expBarContainer"><div id="expBar"></div></div>
  <div id="taskBar">Görevler yükleniyor...</div>
  <div id="screenContent" tabindex="0">Maden ekranında kazmak için herhangi bir yere tıklayın...</div>
  <div id="bottomBar">
    <div class="bottomBtn" id="btnMine"><span><i class="fa-solid fa-gavel"></i></span>Maden</div>
    <div class="bottomBtn" id="btnShop"><span><i class="fa-solid fa-store"></i></span>Mağaza</div>
    <div class="bottomBtn" id="btnCraft"><span><i class="fa-solid fa-screwdriver-wrench"></i></span>Craft</div>
    <div class="bottomBtn" id="btnInventory"><span><i class="fa-solid fa-bag-shopping"></i></span>Envanter</div>
    <div class="bottomBtn" id="btnTasks"><span><i class="fa-solid fa-list-check"></i></span>Görevler</div>
    <div class="bottomBtn" id="btnMarket"><span><i class="fa-solid fa-chart-line"></i></span>Borsa</div>
  </div>

  <div id="btnBankTop"><i class="fa-solid fa-building-columns"></i></div>
  <div id="btnAdminTop"><i class="fa-solid fa-user-secret"></i></div>

  <div id="bankModal">
    <div id="bankContent">
      <button class="close-button" onclick="closeBankModal()">&times;</button>
      <h2>Banka</h2>
      <p style="text-align:center;"><b>Coin:</b> <span id="bankCoinValue">0</span> | <b>TL Karşılığı:</b> <span id="bankTlValue">0.00 TL</span></p>
      <br>
      <h3>Para Çekme (Simülasyon)</h3>
      <label for="ibanInput">IBAN:</label>
      <input type="text" id="ibanInput" placeholder="TRxx xxxx xxxx xxxx xxxx xxxx xx"><br>
      <label for="ibanOwnerInput">IBAN Sahibi:</label>
      <input type="text" id="ibanOwnerInput" placeholder="Ad Soyad"><br>
      <label for="withdrawAmount">Tutar (Coin):</label>
      <input type="number" id="withdrawAmount" min="1" value="1"><br>
      <button onclick="withdrawCoin()">Para Çek</button>

      <br><br>
      <h3>Para Yatırma (Simülasyon)</h3>
      <label for="depositIban">IBAN:</label>
      <input type="text" id="depositIban" value="TR90 0011 1000 0000 0118 8170 01" readonly><br>
      <label for="depositName">İsim Soyisim:</label>
      <input type="text" id="depositName" value="Burak Güleşci" readonly><br>
      <label for="depositAmount">Yatırım Tutarı (TL):</label>
      <input type="number" id="depositAmount" min="1" value="100"><br>
      <button onclick="depositCoin()">Onayla</button>
    </div>
  </div>

  <div id="saleDetailModal">
    <div id="saleDetailModalContent">
        <button class="close-button" onclick="closeSaleDetailModal()">&times;</button>
        <h2>Satış Detayları</h2>
        <p>Satılacak Ürün: <b id="saleItemName"></b></p>
        <p>Adet: <b id="saleItemQtyDisplay"></b></p>
        <label for="saleQuantityInput">Kaç adet satmak istersin?</label>
        <input type="number" id="saleQuantityInput" min="1" value="1">
        <p>Tahmini Kazanç: <b id="estimatedSaleValue">0 Coin</b></p>
        <div class="button-group">
            <button class="sell-button" id="confirmSaleButton">Satışı Onayla</button>
            <button onclick="closeSaleDetailModal()">İptal</button>
        </div>
    </div>
  </div>

  <div id="purchaseDetailModal">
    <div id="purchaseDetailModalContent">
        <button class="close-button" onclick="closePurchaseDetailModal()">&times;</button>
        <h2>Satın Alma Detayları</h2>
        <p>Satın Alınacak Ürün: <b id="purchaseItemName"></b></p>
        <p>Birim Fiyat: <b id="purchaseItemUnitPrice">0 Coin</b></p>
        <label for="purchaseQuantityInput">Kaç adet almak istersin?</label>
        <input type="number" id="purchaseQuantityInput" min="1" value="1">
        <p>Toplam Maliyet: <b id="estimatedPurchaseValue">0 Coin</b></p>
        <div class="button-group">
            <button class="purchase-button" id="confirmPurchaseButton">Satın Almayı Onayla</button>
            <button onclick="closePurchaseDetailModal()">İptal</button>
        </div>
    </div>
  </div>

  <div id="marketModal">
    <div id="marketContent">
      <button class="close-button" onclick="closeMarketModal()">&times;</button>
      <h2>Borsa</h2>
      <p style="font-size: 11px; text-align: center; color: var(--border-color); margin-bottom: 15px;">Fiyatlar her 30 saniyede bir güncellenir.</p>
      <div id="marketItems">
        </div>
    </div>
  </div>

  <div id="adminModal">
    <div id="adminContent">
      <button class="close-button" onclick="closeAdminModal()">&times;</button>
      <h2>Admin Paneli</h2>
      <h3>Coin Gönder</h3>
      <label for="targetUserId">Hedef Kullanıcı ID:</label>
      <input type="text" id="targetUserId" placeholder="Kullanıcı ID (örn: 123456)"><br>
      <label for="sendCoinAmount">Gönderilecek Coin:</label>
      <input type="number" id="sendCoinAmount" min="1" value="100"><br>
      <button onclick="sendCoinToUser()">Coin Gönder</button>
      <p style="font-size: 11px; text-align: center; color: var(--border-color); margin-top: 15px;">
        Not: Bu sadece bir simülasyondur ve Coin gerçek bir kullanıcıya gönderilmez.
      </p>
    </div>
  </div>

</div>

<audio id="mineSound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_8a0bdfdbe7.mp3?filename=stone-pickaxe-104196.mp3" preload="auto"></audio>

<script>
(() => {
  // --- VERİ YAPILARI ---

  // Tüm itemler (ikonlar Font Awesome ile temsil edilecek)
  const allItems = {
    "Taş": { value: 15, desc: "Değerli taş", icon: "fa-solid fa-gem" },
    "Gümüş": { value: 40, desc: "Gümüş cevheri", icon: "fa-solid fa-grip-lines" },
    "Altın": { value: 80, desc: "Değerli altın", icon: "fa-solid fa-coins" },
    "Elmas": { value: 120, desc: "Nadir elmas", icon: "fa-solid fa-diamond" },
    "Pırlanta": { value: 3400, desc: "Çok nadir pırlanta", icon: "fa-solid fa-gem-heart" },
    "Taş Kazma": { value: 3500, type: "tool", durability: 80, desc: "Başlangıç taş kazması", icon: "fa-solid fa-gavel" },
    "Gümüş Kazma": { value: 5000, type: "tool", durability: 120, desc: "Daha güçlü gümüş kazma", icon: "fa-solid fa-gavel" },
    "Altın Kazma": { value: 15000, type: "tool", durability: 200, desc: "Altın kazma", icon: "fa-solid fa-gavel" },
    "Elmas Kazma": { value: 50000, type: "tool", durability: 350, desc: "Elmas kazma", icon: "fa-solid fa-gavel" },
    "Energy Drink": { value: 1000, type: "consumable", energy: 50, desc: "Enerji içeceği +50 enerji verir.", icon: "fa-solid fa-bolt" }
  };

  // Kazma craft tarifleri
  const craftRecipes = {
    "Gümüş Kazma": { "Gümüş": 25 },
    "Altın Kazma": { "Altın": 60 },
    "Elmas Kazma": { "Elmas": 100 }
  };

  // Oyun durumu
  let state = {
    coin: 0,
    energy: 100,
    level: 1,
    exp: 0,
    bag: [],
    equippedPickaxe: "Taş Kazma",
    equippedPickaxeDurability: 80,
    lastEnergyUpdateTime: Date.now(),
    lastTaskRefreshTime: Date.now(),
    marketPrices: {
        "Altın": { current: 80, base: 80 },
        "Gümüş": { current: 40, base: 40 },
        "Elmas": { current: 120, base: 120 },
        "Pırlanta": { current: 3400, base: 3400 }
    },
    playerID: "789012345" // Oyuncu ID'si eklendi
  };

  // Görev havuzu (rastgele görevler için)
  const taskTemplates = [
      { text: "{count} {item} kaz", type: "mine", items: ["Taş", "Gümüş", "Altın"], counts: [5, 10, 15, 20], rewards: {coin: [25, 50, 75], exp: [10, 20, 30]} },
      { text: "{count} {item} sat", type: "sell", items: ["Taş", "Gümüş", "Altın"], counts: [5, 10, 15], rewards: {coin: [30, 60, 90], exp: [15, 25, 35]} },
  ];

  let tasks = [];
  let dailyTask = {};

  // Satış pop-up'ı için geçici değişkenler
  let currentSaleItem = null;
  let currentSaleItemQty = 0;

  // Satın alma pop-up'ı için geçici değişkenler
  let currentPurchaseItem = null;
  let currentPurchaseItemPrice = 0;

  // Admin panel şifresi ve tıklama sayacı
  const ADMIN_PASSWORD_CLICKS = 3;
  let adminClickCount = 0;
  let adminAccessEnabled = false; // Admin erişimi etkin mi?

  // --- HTML Elementleri Referansları ---
  const elEnergy = document.getElementById("energy");
  const elCurrentPickaxeDurability = document.getElementById("currentPickaxeDurability");
  const elLevel = document.getElementById("level");
  const elPickaxeType = document.getElementById("pickaxeType");
  const elTlValue = document.getElementById("tlValue");
  const elCoinValue = document.getElementById("coinValue");
  const elExpBar = document.getElementById("expBar");
  const elTaskBar = document.getElementById("taskBar");
  const screenContent = document.getElementById("screenContent");
  const bankModal = document.getElementById("bankModal");
  const bankCoinValue = document.getElementById("bankCoinValue");
  const bankTlValue = document.getElementById("bankTlValue");
  const saleDetailModal = document.getElementById("saleDetailModal");
  const saleItemName = document.getElementById("saleItemName");
  const saleItemQtyDisplay = document.getElementById("saleItemQtyDisplay");
  const saleQuantityInput = document.getElementById("saleQuantityInput");
  const estimatedSaleValue = document.getElementById("estimatedSaleValue");
  const confirmSaleButton = document.getElementById("confirmSaleButton");
  const purchaseDetailModal = document.getElementById("purchaseDetailModal");
  const purchaseItemName = document.getElementById("purchaseItemName");
  const purchaseItemUnitPrice = document.getElementById("purchaseItemUnitPrice");
  const purchaseQuantityInput = document.getElementById("purchaseQuantityInput");
  const estimatedPurchaseValue = document.getElementById("estimatedPurchaseValue");
  const confirmPurchaseButton = document.getElementById("confirmPurchaseButton");
  const marketModal = document.getElementById("marketModal");
  const marketItemsContainer = document.getElementById("marketItems");
  const elTelegramUserInfo = document.getElementById("telegramUserInfo");
  const btnAdminTop = document.getElementById("btnAdminTop"); // Admin butonu
  const adminModal = document.getElementById("adminModal"); // Admin modal

  // --- KAYDET & YÜKLE ---

  function saveGame() {
    const saveData = {
      state,
      tasks,
      dailyTask
    };
    localStorage.setItem("bbg-metin-rpg-save", JSON.stringify(saveData));
  }

  function loadGame() {
    const saveData = JSON.parse(localStorage.getItem("bbg-metin-rpg-save"));
    if (saveData) {
      state = saveData.state;
      if (typeof state.equippedPickaxeDurability === 'undefined') {
          state.equippedPickaxeDurability = allItems[state.equippedPickaxe]?.durability || 80;
      }
      if (!saveData.tasks || saveData.tasks.length === 0) {
        tasks = [];
        for (let i = 0; i < 5; i++) {
            tasks.push(generateRandomTask());
        }
        dailyTask = generateRandomTask(true);
        dailyTask.text = "GÜNLÜK GÖREV: " + dailyTask.text;
        state.lastTaskRefreshTime = Date.now();
      } else {
        tasks = saveData.tasks;
        dailyTask = saveData.dailyTask;
      }
      state.bag.forEach(item => {
          if (allItems[item.name]?.type === "tool" && typeof item.durability === 'undefined') {
              item.durability = allItems[item.name].durability;
          }
      });
      state.bag.forEach(item => {
          if (item.name === "Demir") item.name = "Gümüş";
          if (item.name === "Demir Kazma") item.name = "Gümüş Kazma";
      });
      state.equippedPickaxe = state.equippedPickaxe === "Demir Kazma" ? "Gümüş Kazma" : state.equippedPickaxe;

      if (!state.marketPrices) {
        state.marketPrices = {
            "Altın": { current: 80, base: 80 },
            "Gümüş": { current: 40, base: 40 },
            "Elmas": { current: 120, base: 120 },
            "Pırlanta": { current: 3400, base: 3400 }
        };
      }
      if (!state.marketPrices["Pırlanta"]) {
          state.marketPrices["Pırlanta"] = { current: 3400, base: 3400 };
      }
      // Player ID'yi yükle, yoksa varsayılanı kullan
      if (!state.playerID) {
          state.playerID = generatePlayerId(); // İlk defa oyun yüklendiğinde ID ata
      }
    } else {
      // Yeni oyun ise oyuncu ID'si oluştur
      state.playerID = generatePlayerId();
    }
  }

  // --- YARDIMCI FONKSİYONLAR ---

  // Rastgele oyuncu ID'si oluşturma (sadece simülasyon için)
  function generatePlayerId() {
      return Math.floor(100000000 + Math.random() * 900000000).toString();
  }

  // Binlik değerleri kısaltma fonksiyonu (örn: 1500 -> 1.5k)
  function formatNumber(num) {
      if (num >= 1000) {
          return (num / 1000).toFixed(1).replace(/\.0$/, '') + 'k';
      }
      return num.toString();
  }

  // UI'ı minimum güncelleme ile yenile (sadece topbar ve exp bar)
  function updateUI() {
    elEnergy.textContent = Math.floor(state.energy);
    elCurrentPickaxeDurability.innerHTML = `<i class="fa-solid fa-heart"></i>${Math.floor(state.equippedPickaxeDurability)}`;
    elLevel.textContent = state.level;
    elPickaxeType.textContent = state.equippedPickaxe;
    elTlValue.textContent = formatNumber(state.coin / 1000) + " TL";
    elCoinValue.textContent = formatNumber(state.coin);

    let neededExp = Math.floor(state.level * 120 * Math.pow(1.1, state.level-1));
    let expPercent = Math.min(100, (state.exp / neededExp) * 100);
    elExpBar.style.width = expPercent + "%";

    elTelegramUserInfo.textContent = `@OyuncuAdi (ID: ${state.playerID})`; // Player ID'yi buraya yansıt

    updateTaskBar();
  }

  function showNotice(text) {
    const el = document.createElement("div");
    el.textContent = text;
    el.classList.add("notice");
    document.body.appendChild(el);
    setTimeout(() => el.remove(), 3500);
  }

  function showSmallPopup(text) {
    const el = document.createElement("div");
    el.textContent = text;
    el.classList.add("small-popup");
    document.body.appendChild(el);
    setTimeout(() => el.remove(), 2500);
  }

  // Seviye atlama kontrolü
  function checkLevelUp() {
    let neededExp = Math.floor(state.level * 120 * Math.pow(1.1, state.level-1));
    while (state.exp >= neededExp) {
      state.exp -= neededExp;
      state.level++;
      state.energy = 100;
      state.equippedPickaxeDurability = allItems[state.equippedPickaxe]?.durability || 80;
      showNotice(`🎉 Seviye atladın! Yeni seviye: ${state.level}`);
      neededExp = Math.floor(state.level * 120 * Math.pow(1.1, state.level-1));
    }
    updateUI();
  }

  // Görev barını güncelle
  function updateTaskBar() {
    let incompleteTasks = tasks.filter(t => !t.completed);
    let currentTask = incompleteTasks.length > 0 ? incompleteTasks[0] : dailyTask.completed ? null : dailyTask;
    if (!currentTask) {
      elTaskBar.textContent = "Tüm görevler tamamlandı! Yeni görevler yakında.";
      return;
    }
    let progress = currentTask.done + "/" + currentTask.count;
    let rewardCoin = formatNumber(currentTask.coinReward);
    elTaskBar.textContent = `🎯 Görev: ${currentTask.text} (${progress}) - Ödül: ${rewardCoin} Coin`;
  }

  // Görev ilerlemesini kontrol et
  function updateTaskProgress(item, type) {
    let taskCompleted = false;
    for(let t of tasks){
      if(!t.completed && t.item === item && t.type === type){
        t.done++;
        if(t.done >= t.count){
          t.completed = true;
          state.coin += t.coinReward;
          state.exp += t.expReward;
          showNotice(`✅ Görev tamamlandı: ${t.text}`);
          taskCompleted = true;
          checkLevelUp();
        }
      }
    }
    if(!dailyTask.completed && dailyTask.item === item && dailyTask.type === type){
      dailyTask.done++;
      if(dailyTask.done >= dailyTask.count){
        dailyTask.completed = true;
        state.coin += dailyTask.coinReward;
        state.exp += dailyTask.expReward;
        showNotice(`✅ Günlük görev tamamlandı: ${dailyTask.text}`);
        taskCompleted = true;
        checkLevelUp();
      }
    }
    updateTaskBar();
    if(taskCompleted) saveGame();
  }

  // Rastgele integer
  function randomInt(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }

  // Yeni görev oluşturma
  function generateRandomTask(isDaily = false) {
      const template = taskTemplates[randomInt(0, taskTemplates.length - 1)];
      const item = template.items[randomInt(0, template.items.length - 1)];
      const count = template.counts[randomInt(0, template.counts.length - 1)];
      let coinReward = template.rewards.coin[randomInt(0, template.rewards.coin.length - 1)];
      const expReward = template.rewards.exp[randomInt(0, template.rewards.exp.length - 1)];

      if (isDaily) {
          coinReward = Math.max(500, coinReward * 2);
      }

      return {
          id: isDaily ? 6 : tasks.length + 1,
          text: template.text.replace("{count}", count).replace("{item}", item),
          item: item,
          count: count,
          done: 0,
          coinReward: coinReward,
          expReward: expReward * (isDaily ? 2 : 1),
          completed: false,
          type: template.type
      };
  }

  // Görevleri yenileme
  function refreshTasks() {
      const now = Date.now();
      const oneDay = 24 * 60 * 60 * 1000;

      if (now - state.lastTaskRefreshTime >= oneDay) {
          tasks = [];
          for (let i = 0; i < 5; i++) {
              tasks.push(generateRandomTask());
          }
          dailyTask = generateRandomTask(true);
          dailyTask.text = "GÜNLÜK GÖREV: " + dailyTask.text;
          state.lastTaskRefreshTime = now;
          showNotice("✅ Yeni görevler yüklendi!");
          saveGame();
          updateTaskBar();
      }
  }

  // Enerji yenileme fonksiyonu (Sayaç gibi ilerleme)
  function updateEnergyLoop() {
    const now = Date.now();
    const timeElapsedSinceLastUpdate = now - state.lastEnergyUpdateTime;
    const energyPerSecond = 100 / (30 * 60);

    if (state.energy < 100) {
      const potentialEnergyToAdd = (timeElapsedSinceLastUpdate / 1000) * energyPerSecond;
      state.energy = Math.min(100, state.energy + potentialEnergyToAdd);
      elEnergy.textContent = Math.floor(state.energy);
      state.lastEnergyUpdateTime = now;
      saveGame();
    } else if (state.energy >= 100 && state.lastEnergyUpdateTime < now - (30 * 60 * 1000)) {
      state.lastEnergyUpdateTime = now;
      saveGame();
    }
  }

  // --- OYUN MEKANİKLERİ ---

  // Kazma işlemi (sadece Maden ekranında geçerli)
  let clickCount = 0;
  let rewardClickCount = randomInt(8,12);

  function mineClick(e){
    if(currentScreen !== "mine") return;

    if(state.energy <= 0) {
      showNotice("⚡ Enerjin bitti! Mağazadan enerji içeceği alabilirsin.");
      return;
    }
    if(state.equippedPickaxeDurability <= 0) {
      showNotice("🛠 Kazman kırıldı! Envanterden tamir et veya yenisini al.");
      return;
    }

    // Küçük rastgele coin kazanımı
    let minCoin = 17;
    let maxCoin = 150;
    let coinGain = Math.floor(Math.random() * (Math.random() < 0.7 ? (maxCoin * 0.3) : maxCoin)) + minCoin;
    state.coin += coinGain;
    showCoinDropText(`+${coinGain} Coin!`);

    // Item düşme şansı
    clickCount++;
    if(clickCount >= rewardClickCount) {
      clickCount = 0;
      rewardClickCount = randomInt(8,12);

      let itemPool = ["Taş","Gümüş","Altın","Elmas","Pırlanta"];
      let chances = {
        "Taş": 0.45,
        "Gümüş": 0.30,
        "Altın": 0.18,
        "Elmas": 0.06 + 0.01*state.level,
        "Pırlanta": 0.01 + 0.005*state.level
      };

      let rand = Math.random();
      let acc=0;
      let dropped=null;
      for(let it of itemPool){
        acc += chances[it];
        if(rand <= acc){
          dropped = it;
          break;
        }
      }
      if(dropped){
        addItem(dropped,1);
        updateTaskProgress(dropped, "mine");
        animateDrop(e, allItems[dropped].icon || "fa-solid fa-question");
        showItemDropText(`+1 ${dropped}!`);
      }
    }

    // Kazma dayanıklılığı ve enerji azalması (level zorluk arttıkça daha hızlı azalabilir)
    let durabilityLoss = 1 + state.level*0.05;
    let energyLoss = 1 + state.level*0.03;
    state.equippedPickaxeDurability = Math.max(0, state.equippedPickaxeDurability - durabilityLoss);
    state.energy = Math.max(0, state.energy - energyLoss);

    // Exp kazanımı
    let expGain = 5 + Math.floor(state.level/2);
    state.exp += expGain;
    checkLevelUp();

    if (state.exp < Math.floor(state.level * 120 * Math.pow(1.1, state.level-1))) {
        let neededExp = Math.floor(state.level * 120 * Math.pow(1.1, state.level-1));
        let expPercent = Math.min(100, (state.exp / neededExp) * 100);
        elExpBar.style.width = expPercent + "%";
    }
    updateUI();

    saveGame();
    document.getElementById("mineSound").currentTime = 0;
    document.getElementById("mineSound").play();
  }

  // Animasyonlu item drop gösterimi
  function animateDrop(e, itemIconClass) {
    let el = document.createElement("i");
    el.className = itemIconClass;
    el.classList.add("mine-icon");
    const gameWrapperRect = document.getElementById("gameWrapper").getBoundingClientRect();
    el.style.left = (e.clientX - gameWrapperRect.left) + "px";
    el.style.top = (e.clientY - gameWrapperRect.top) + "px";
    screenContent.appendChild(el);
    setTimeout(() => el.remove(), 900);
  }

  function showItemDropText(text) {
      const el = document.createElement("div");
      el.textContent = text;
      el.classList.add("item-drop-text");
      screenContent.appendChild(el);
      setTimeout(() => el.remove(), 1500);
  }

  function showCoinDropText(text) {
      const el = document.createElement("div");
      el.textContent = text;
      el.classList.add("coin-drop-text");
      screenContent.appendChild(el);
      setTimeout(() => el.remove(), 1500);
  }

  // Envantere item ekle
  function addItem(name, qty) {
    let slot = state.bag.find(i => i.name === name);
    if(slot) {
      slot.qty += qty;
    } else {
      if (allItems[name].type === "tool") {
          state.bag.push({name, qty: 1, durability: allItems[name].durability});
      } else {
          state.bag.push({name, qty});
      }
    }
  }

  // --- EKRANLAR ---

  let currentScreen = "mine";
  let currentScreenHtml = "";

  function switchScreen(screenHtml, newScreenName) {
      closeBankModal();
      closeSaleDetailModal();
      closeMarketModal();
      closePurchaseDetailModal();
      closeAdminModal(); // Admin modalı da kapat

      if (newScreenName === "mine") {
          screenContent.classList.add("mine-screen");
      } else {
          screenContent.classList.remove("mine-screen");
      }

      if (currentScreenHtml !== screenHtml || currentScreen !== newScreenName) {
          screenContent.style.opacity = 0;
          setTimeout(() => {
              screenContent.innerHTML = screenHtml;
              currentScreen = newScreenName;
              currentScreenHtml = screenHtml;
              screenContent.style.opacity = 1;
          }, 300);
      }
  }

  function showMineScreen() {
    const html = `<p style="text-align:center;">Maden ekranında kazmak için herhangi bir yere tıklayın.</p><br>` +
      `<p style="text-align:center;"><b>Enerji ve Kazma Dayanıklılığını iyi takip et!</b></p>`;
    switchScreen(html, "mine");
  }

  function showShopScreen() {
    let html = `<h2>Mağaza</h2>`;

    html += `<h3>Tüketilebilirler</h3>`;
    let energyDrinkPrice = allItems["Energy Drink"].value;
    html += `<div class="item-row">
        <div><i class="${allItems["Energy Drink"].icon}"></i> Energy Drink</div>
        <div><b>${formatNumber(energyDrinkPrice)} Coin</b></div>
        <div class="item-actions"><button onclick="openPurchaseDetailModal('Energy Drink')">Satın Al</button></div>
      </div>`;

    html += `<h3>Kazmalar</h3>`;
    if (allItems["Taş Kazma"].value > 0) {
        let price = allItems["Taş Kazma"].value;
        html += `<div class="item-row">
            <div><i class="${allItems["Taş Kazma"].icon}"></i> Taş Kazma</div>
            <div><b>${formatNumber(price)} Coin</b></div>
            <div class="item-actions"><button onclick="openPurchaseDetailModal('Taş Kazma')">Satın Al</button></div>
        </div>`;
    }
    for(let key of Object.keys(craftRecipes)){
      let price = allItems[key].value;
      if (price === 0) continue;
      html += `<div class="item-row">
        <div><i class="${allItems[key].icon}"></i> ${key}</div>
        <div><b>${formatNumber(price)} Coin</b></div>
        <div class="item-actions"><button onclick="openPurchaseDetailModal('${key}')">Satın Al</button></div>
      </div>`;
    }
    switchScreen(html, "shop");
  }

  function showCraftScreen() {
    let html = `<h2>Craft Ekranı</h2><h3>Malzemelerin:</h3><div>`;
    for(let item in craftRecipes){
      html += `<div class="craft-item"><strong><i class="${allItems[item].icon}"></i> ${item}</strong> — Gereken: `;
      let needed = craftRecipes[item];
      let parts = [];
      for(let mat in needed){
        let owned = (state.bag.find(b => b.name === mat)?.qty) || 0;
        let enough = owned >= needed[mat];
        parts.push(`<span style="color:${enough ? "#00ffaa" : "#ff4d4d"}">${mat} (${owned}/${needed[mat]})</span>`);
      }
      html += `<div>${parts.join(", ")}</div> <button onclick="craftItem('${item}')" ${canCraft(item) ? "" : "disabled"}>Craft</button></div>`;
    }
    html += `</div>`;
    switchScreen(html, "craft");
  }

  function canCraft(item){
    if(!craftRecipes[item]) return false;
    let needed = craftRecipes[item];
    for(let mat in needed){
      let owned = (state.bag.find(b => b.name === mat)?.qty) || 0;
      if(owned < needed[mat]) return false;
    }
    return true;
  }

  window.craftItem = function(item) {
    if(!canCraft(item)) {
        showNotice("Yeterli malzemen yok!");
        return;
    }
    let needed = craftRecipes[item];
    for(let mat in needed){
      let slot = state.bag.find(b => b.name === mat);
      slot.qty -= needed[mat];
      if(slot.qty <= 0) {
        state.bag = state.bag.filter(b => b.qty > 0);
      }
    }
    addItem(item, 1);
    showNotice(`${item} üretildi!`);
    saveGame();
    updateUI();
    showCraftScreen();
  }

  function showInventoryScreen() {
    let allInventoryItems = [...state.bag];

    allInventoryItems.sort((a, b) => {
        if (a.name === state.equippedPickaxe) return -1;
        if (b.name === state.equippedPickaxe) return 1;

        const typeA = allItems[a.name]?.type;
        const typeB = allItems[b.name]?.type;
        if (typeA === "tool" && typeB !== "tool") return -1;
        if (typeA !== "tool" && typeB === "tool") return 1;

        if (typeA === "consumable" && typeB !== "consumable") return -1;
        if (typeA !== "consumable" && typeB === "consumable") return 1;

        return 0;
    });

    let html = "<h2>Envanter</h2>";
    if(allInventoryItems.length === 0) {
      html += "<p>Envanter boş.</p>";
      switchScreen(html, "inventory");
      return;
    }

    for(let slot of allInventoryItems){
        const itemInfo = allItems[slot.name];
        if (!itemInfo) continue;

        let useButtonHtml = '';
        let repairButtonHtml = '';
        let durabilityDisplay = '';
        let isEquipped = (state.equippedPickaxe === slot.name);

        let displayQty = slot.qty;

        if (itemInfo.type === "consumable" && slot.name === "Energy Drink") {
            useButtonHtml = `<button class="use-button" onclick="useItem('${slot.name}')">Kullan</button>`;
        } else if (itemInfo.type === "tool") {
            const maxDurability = itemInfo.durability;
            let currentDurability = slot.durability;

            if (isEquipped) {
                currentDurability = state.equippedPickaxeDurability;
                durabilityDisplay = `<span style="color:var(--neon-green-heart);"><i class="fa-solid fa-heart"></i>%${Math.floor((currentDurability / maxDurability) * 100)}</span>`;
                useButtonHtml = `<button class="use-button" disabled>Kuşanılı</button>`;
            } else {
                durabilityDisplay = `Canı: <i class="fa-solid fa-heart"></i>%${Math.floor((currentDurability / maxDurability) * 100)}`;
                useButtonHtml = `<button class="use-button" onclick="useItem('${slot.name}')">Kuşan</button>`;
            }

            if (currentDurability < maxDurability) {
                repairButtonHtml = `<button class="repair-button" ${state.coin >= 2000 ? "" : "disabled"} onclick="repairPickaxe('${slot.name}', ${isEquipped})">Tamir Et</button>`;
            }
        }

        let sellButtonHtml = `<button class="sell-button" onclick="openSaleDetailModal('${slot.name}')">Sat</button>`;

        html += `<div class="item-row">
            <div><i class="${itemInfo?.icon || "fa-solid fa-question"}"></i> ${slot.name} ${durabilityDisplay}</div>
            <div>Adet: ${displayQty}</div>
            <div class="item-actions">
                ${sellButtonHtml}
                ${useButtonHtml}
                ${repairButtonHtml}
            </div>
        </div>`;
    }

    switchScreen(html, "inventory");
  }

  const SELL_PERCENTAGE = 1;

  window.openSaleDetailModal = function(itemName) {
      const slot = state.bag.find(b => b.name === itemName);

      if (!slot) {
          showNotice("Bu eşyaya sahip değilsin!");
          return;
      }

      if (itemName === state.equippedPickaxe && allItems[itemName].type === "tool" && slot.qty === 1) {
          showNotice("Kuşanılı kazman envanterdeki son kazman. Satamazsın!");
          return;
      }

      currentSaleItem = itemName;
      currentSaleItemQty = slot.qty;

      saleItemName.textContent = itemName;
      saleItemQtyDisplay.textContent = currentSaleItemQty;
      saleQuantityInput.value = 1;
      saleQuantityInput.max = currentSaleItemQty;

      updateEstimatedSaleValue();

      saleDetailModal.classList.add("active");
  };

  window.closeSaleDetailModal = function() {
      saleDetailModal.classList.remove("active");
      currentSaleItem = null;
      currentSaleItemQty = 0;
  };

  function updateEstimatedSaleValue() {
      const qtyToSell = parseInt(saleQuantityInput.value);
      if (isNaN(qtyToSell) || qtyToSell < 1) {
          estimatedSaleValue.textContent = "Geçersiz Adet";
          confirmSaleButton.disabled = true;
          return;
      }
      if (qtyToSell > currentSaleItemQty) {
          estimatedSaleValue.textContent = "Yetersiz Adet";
          confirmSaleButton.disabled = true;
          return;
      }

      const marketInfo = state.marketPrices[currentSaleItem];
      const baseValue = marketInfo ? marketInfo.current : (allItems[currentSaleItem]?.value || 0);

      const calculatedSalePrice = Math.floor(baseValue * SELL_PERCENTAGE * qtyToSell);
      estimatedSaleValue.textContent = `${formatNumber(calculatedSalePrice)} Coin`;
      confirmSaleButton.disabled = false;
  }

  saleQuantityInput.addEventListener("input", updateEstimatedSaleValue);

  confirmSaleButton.addEventListener("click", () => {
      const qtyToSell = parseInt(saleQuantityInput.value);
      if (isNaN(qtyToSell) || qtyToSell < 1 || qtyToSell > currentSaleItemQty) {
          showNotice("Geçersiz satış adedi!");
          return;
      }

      const slot = state.bag.find(b => b.name === currentSaleItem);
      if (currentSaleItem === state.equippedPickaxe && allItems[currentSaleItem].type === "tool" && slot.qty === 1) {
        showNotice("Kuşanılı kazman envanterdeki son kazman. Satamazsın!");
        closeSaleDetailModal();
        return;
      }

      const marketInfo = state.marketPrices[currentSaleItem];
      const baseValue = marketInfo ? marketInfo.current : (allItems[currentSaleItem]?.value || 0);

      const sellPrice = Math.floor(baseValue * SELL_PERCENTAGE * qtyToSell);

      if (slot) {
          slot.qty -= qtyToSell;
          if(slot.qty <= 0) state.bag = state.bag.filter(b => b.qty > 0);
      } else {
          showNotice("Bir hata oluştu, lütfen tekrar deneyin. (SELL ERROR)");
          closeSaleDetailModal();
          return;
      }

      state.coin += sellPrice;

      showNotice(`${qtyToSell} ${currentSaleItem} satıldı, +${formatNumber(sellPrice)} Coin kazandın.`);
      showSmallPopup(`${currentToSell} x${qtyToSell} = ${formatNumber(sellPrice)} Coin`);
      updateTaskProgress(currentSaleItem, "sell");
      saveGame();
      updateUI();
      closeSaleDetailModal();
      showInventoryScreen();
  });

  window.useItem = function(itemName) {
    const itemInfo = allItems[itemName];
    const slot = state.bag.find(b => b.name === itemName);

    if (!slot) {
      showNotice("Bu eşyaya sahip değilsin!");
      return;
    }

    if (itemInfo.type === "consumable" && itemName === "Energy Drink") {
      if (state.energy === 100) {
        showNotice("Enerjin zaten tam!");
        return;
      }
      state.energy = 100;
      slot.qty--;
      if (slot.qty <= 0) state.bag = state.bag.filter(b => b.qty > 0);
      showNotice("Enerjin tamamen yenilendi!");
    } else if (itemInfo.type === "tool") {
      if (state.equippedPickaxe === itemName) {
          showNotice("Bu kazma zaten kuşanılı!");
          return;
      }

      let currentEquippedSlot = state.bag.find(b => b.name === state.equippedPickaxe);
      if (currentEquippedSlot) {
          currentEquippedSlot.durability = state.equippedPickaxeDurability;
      }

      state.equippedPickaxe = itemName;
      state.equippedPickaxeDurability = slot.durability;
      showNotice(`${itemName} kuşanıldı!`);
    } else {
      showNotice("Bu eşya kullanılamaz.");
      return;
    }

    saveGame();
    updateUI();
    showInventoryScreen();
  };

  window.repairPickaxe = function(pickaxeName, isEquippedFromCall) {
    const REPAIR_COST = 2000;
    const maxDurability = allItems[pickaxeName].durability;

    if (state.coin < REPAIR_COST) {
        showNotice("Yeterli Coin yok! Tamir için " + formatNumber(REPAIR_COST) + " Coin gerekli.");
        return;
    }

    let pickaxeSlot = state.bag.find(b => b.name === pickaxeName && allItems[b.name]?.type === "tool");
    if (!pickaxeSlot) {
        showNotice("Tamir edilecek kazma bulunamadı!");
        return;
    }

    let isCurrentlyEquipped = (state.equippedPickaxe === pickaxeName);

    let currentDurability;
    if (isCurrentlyEquipped) {
        currentDurability = state.equippedPickaxeDurability;
    } else {
        currentDurability = pickaxeSlot.durability;
    }

    if (currentDurability >= maxDurability) {
        showNotice("Bu kazmanın tamire ihtiyacı yok!");
        return;
    }

    state.coin -= REPAIR_COST;
    if (isCurrentlyEquipped) {
        state.equippedPickaxeDurability = maxDurability;
    } else {
        pickaxeSlot.durability = maxDurability;
    }

    showNotice(`${pickaxeName} tamir edildi! (-${formatNumber(REPAIR_COST)} Coin)`);
    saveGame();
    updateUI();
    showInventoryScreen();
  };

  window.openPurchaseDetailModal = function(itemName) {
      currentPurchaseItem = itemName;
      currentPurchaseItemPrice = allItems[itemName].value;

      purchaseItemName.textContent = itemName;
      purchaseItemUnitPrice.textContent = `${formatNumber(currentPurchaseItemPrice)} Coin`;
      purchaseQuantityInput.value = 1;
      purchaseQuantityInput.max = Math.floor(state.coin / currentPurchaseItemPrice) || 1;
      if(purchaseQuantityInput.max === 0 && state.coin < currentPurchaseItemPrice) {
          purchaseQuantityInput.value = 0;
          confirmPurchaseButton.disabled = true;
      }

      updateEstimatedPurchaseValue();

      purchaseDetailModal.classList.add("active");
  };

  window.closePurchaseDetailModal = function() {
      purchaseDetailModal.classList.remove("active");
      currentPurchaseItem = null;
      currentPurchaseItemPrice = 0;
  };

  function updateEstimatedPurchaseValue() {
      const qtyToBuy = parseInt(purchaseQuantityInput.value);
      if (isNaN(qtyToBuy) || qtyToBuy < 1) {
          estimatedPurchaseValue.textContent = "Geçersiz Adet";
          confirmPurchaseButton.disabled = true;
          return;
      }
      const totalCost = currentPurchaseItemPrice * qtyToBuy;
      if (totalCost > state.coin) {
          estimatedPurchaseValue.textContent = `Yetersiz Coin (${formatNumber(totalCost)} Coin)`;
          confirmPurchaseButton.disabled = true;
          return;
      }
      estimatedPurchaseValue.textContent = `${formatNumber(totalCost)} Coin`;
      confirmPurchaseButton.disabled = false;
  }

  purchaseQuantityInput.addEventListener("input", updateEstimatedPurchaseValue);

  confirmPurchaseButton.addEventListener("click", () => {
      const qtyToBuy = parseInt(purchaseQuantityInput.value);
      const totalCost = currentPurchaseItemPrice * qtyToBuy;

      if (isNaN(qtyToBuy) || qtyToBuy < 1 || totalCost > state.coin) {
          showNotice("Geçersiz işlem veya yeterli Coin yok!");
          return;
      }

      state.coin -= totalCost;
      addItem(currentPurchaseItem, qtyToBuy);

      if (allItems[currentPurchaseItem].type === "tool") {
          showNotice(`${qtyToBuy} adet ${currentPurchaseItem} satın alındı!`);
      } else {
          showNotice(`${qtyToBuy} adet ${currentPurchaseItem} satın alındı!`);
      }

      showSmallPopup(`${currentPurchaseItem} x${qtyToBuy} = ${formatNumber(totalCost)} Coin`);
      saveGame();
      updateUI();
      closePurchaseDetailModal();
      showShopScreen();
  });

  function showTasksScreen() {
    let html = "<h2>Görevler</h2>";

    html += `<h3>Günlük Görev</h3>`;
    html += `<div class="task-item ${dailyTask.completed ? "completed" : ""}">
      <div>${dailyTask.text}</div>
      <div>Durum: ${dailyTask.done}/${dailyTask.count}</div>
      <div class="task-reward">Ödül: ${formatNumber(dailyTask.coinReward)} Coin, ${dailyTask.expReward} Exp</div>
    </div>`;

    html += `<h3>Diğer Görevler</h3>`;
    if (tasks.length === 0) {
        html += "<p>Şu anda başka görev bulunmamaktadır.</p>";
    } else {
        for(let t of tasks){
          html += `<div class="task-item ${t.completed ? "completed" : ""}">
            <div>${t.text}</div>
            <div>Durum: ${t.done}/${t.count}</div>
            <div class="task-reward">Ödül: ${formatNumber(t.coinReward)} Coin, ${t.expReward} Exp</div>
          </div>`;
        }
    }
    switchScreen(html, "tasks");
  }

  // Banka Modalını Aç
  window.showBankModal = function(){
    bankCoinValue.textContent = formatNumber(state.coin);
    bankTlValue.textContent = formatNumber(state.coin/1000) + " TL";
    document.getElementById("withdrawAmount").value = 1;
    document.getElementById("depositAmount").value = 100;
    bankModal.classList.add("active");
  }

  // Banka Modalını Kapat
  window.closeBankModal = function(){
    bankModal.classList.remove("active");
  }

  window.withdrawCoin = function(){
    let iban = document.getElementById("ibanInput").value.trim();
    let owner = document.getElementById("ibanOwnerInput").value.trim();
    let amount = parseInt(document.getElementById("withdrawAmount").value);
    if(!iban || !owner){
      showNotice("Lütfen IBAN ve sahip bilgilerini girin!");
      return;
    }
    if(isNaN(amount) || amount < 1 || amount > state.coin){
      showNotice("Geçersiz tutar veya yeterli Coin yok!");
      return;
    }
    state.coin -= amount;
    showNotice(`${formatNumber(amount)} Coin çekildi. İşleminiz en kısa sürede işlenecektir.`);
    saveGame();
    updateUI();
    showBankModal();
  }

  window.depositCoin = function(){
    let amount = parseInt(document.getElementById("depositAmount").value);
    if(isNaN(amount) || amount < 1){
      showNotice("Geçersiz yatırım tutarı!");
      return;
    }
    showNotice("Ödemeniz alındı. En kısa sürede hesabınıza yansıtılacaktır.");
  }

  // Borsa ekranını aç
  window.showMarketModal = function() {
      renderMarketPrices();
      marketModal.classList.add("active");
  }

  // Borsa ekranını kapat
  window.closeMarketModal = function() {
marketModal.classList.remove("active");
  }

  // Borsa fiyatlarını güncelle ve render et
  function updateMarketPrices() {
      const itemsToTrack = ["Altın", "Gümüş", "Elmas", "Pırlanta"];
      itemsToTrack.forEach(itemName => {
          let currentPrice = state.marketPrices[itemName].current;
          let basePrice = state.marketPrices[itemName].base;

          let changePercentage = (Math.random() * 0.20) - 0.10;
          let newPrice = Math.round(basePrice * (1 + changePercentage));

          newPrice = Math.max(Math.round(basePrice * 0.7), newPrice);
          newPrice = Math.min(Math.round(basePrice * 1.3), newPrice);

          state.marketPrices[itemName].current = newPrice;
      });
      if (marketModal.classList.contains("active")) {
        renderMarketPrices();
      }
      saveGame();
  }

  function renderMarketPrices() {
      marketItemsContainer.innerHTML = "";
      const itemsToTrack = ["Altın", "Gümüş", "Elmas", "Pırlanta"];

      // Tüm item isimlerinin maksimum uzunluğunu bul (hizalama için)
      const maxNameLength = Math.max(...itemsToTrack.map(name => name.length));

      itemsToTrack.forEach(itemName => {
          const itemData = state.marketPrices[itemName];
          if (!itemData) return;

          const icon = allItems[itemName]?.icon || "fa-solid fa-question";
          const priceChange = itemData.current - itemData.base;
          let arrow = "";
          let priceClass = "";

          if (priceChange > 0) {
              arrow = `<i class="fa-solid fa-caret-up"></i>`;
              priceClass = "price-up";
          } else if (priceChange < 0) {
              arrow = `<i class="fa-solid fa-caret-down"></i>`;
              priceClass = "price-down";
          }

          // Item adını ve fiyatı ayrı ayrı hizala
          const marketItemHtml = `
              <div class="market-item">
                  <span class="item-name"><i class="${icon}"></i> ${itemName}</span>
                  <span class="item-price-group">
                      <span class="item-price ${priceClass}">${formatNumber(itemData.current)}</span>
                      <span class="price-arrow">${arrow}</span>
                  </span>
              </div>
          `;
          marketItemsContainer.innerHTML += marketItemHtml;
      });
  }

  // --- Admin Fonksiyonları ---

  // Admin Modalını Aç
  window.showAdminModal = function() {
      adminModal.classList.add("active");
  }

  // Admin Modalını Kapat
  window.closeAdminModal = function() {
      adminModal.classList.remove("active");
  }

  // Coin Gönderme Simülasyonu
  window.sendCoinToUser = function() {
      const targetId = document.getElementById("targetUserId").value.trim();
      const amount = parseInt(document.getElementById("sendCoinAmount").value);

      if (!targetId || isNaN(amount) || amount < 1) {
          showNotice("Lütfen geçerli bir Kullanıcı ID ve miktar girin!");
          return;
      }

      // Bu bir simülasyon olduğundan, sadece bildirim gösteriyoruz.
      // Gerçek bir oyunda burada sunucu tarafında işlemler olurdu.
      showNotice(`Admin: ${formatNumber(amount)} Coin, ${targetId} ID'li kullanıcıya gönderildi (Simülasyon).`);
      saveGame(); // Sadece bildirim sonrası kaydet
  }

  // --- BUTON EVENTLERİ ---

  document.getElementById("btnMine").addEventListener("click", showMineScreen);
  document.getElementById("btnShop").addEventListener("click", showShopScreen);
  document.getElementById("btnCraft").addEventListener("click", showCraftScreen);
  document.getElementById("btnInventory").addEventListener("click", showInventoryScreen);
  document.getElementById("btnTasks").addEventListener("click", showTasksScreen);
  document.getElementById("btnMarket").addEventListener("click", showMarketModal);

  document.getElementById("btnBankTop").addEventListener("click", showBankModal);
  document.getElementById("btnAdminTop").addEventListener("click", showAdminModal); // Admin butonu event listener

  // Görevler başlığına 3 kere tıklama ile Admin panelini açma
  let tasksHeaderClickCount = 0;
  const tasksHeader = document.querySelector("#tasks h2") || document.createElement('div'); // Fallback for initial load
  // Delegated event listener for dynamic task screen
  document.getElementById("bottomBar").addEventListener("click", function(event) {
    if (event.target.closest("#btnTasks")) {
      // Small delay to ensure the screen content is updated
      setTimeout(() => {
        const currentTasksHeader = screenContent.querySelector("h2");
        if (currentTasksHeader && currentTasksHeader.textContent === "Görevler") {
          currentTasksHeader.removeEventListener('click', handleTasksHeaderClick); // Remove old listener if exists
          currentTasksHeader.addEventListener('click', handleTasksHeaderClick); // Add new listener
        }
      }, 100);
    }
  });


  function handleTasksHeaderClick() {
    tasksHeaderClickCount++;
    if (tasksHeaderClickCount >= ADMIN_PASSWORD_CLICKS) {
      adminAccessEnabled = true;
      btnAdminTop.style.display = 'flex'; // Admin butonunu görünür yap
      showNotice("Admin Paneli Aktif Edildi!");
      tasksHeaderClickCount = 0; // Reset count
      // Remove the event listener to prevent further triggers until screen refresh
      this.removeEventListener('click', handleTasksHeaderClick);
    }
  }

  // --- SAYFA İÇİ ETKİLEŞİMLER ---

  screenContent.addEventListener("click", (e) => {
    if(currentScreen === "mine") {
      mineClick(e);
    }
  });

  // --- BAŞLANGIÇ ---

  function initializeGame() {
    loadGame();

    if (!state.bag.find(item => item.name === "Taş Kazma" && allItems["Taş Kazma"].type === "tool")) {
        addItem("Taş Kazma", 1);
        state.equippedPickaxe = "Taş Kazma";
        state.equippedPickaxeDurability = allItems["Taş Kazma"].durability;
    }

    refreshTasks();
    updateUI();

    showMineScreen();
    setInterval(updateEnergyLoop, 1000);
    setInterval(refreshTasks, 60 * 1000);
    setInterval(updateMarketPrices, 30 * 1000);
  }

  initializeGame();

})();
</script>
</body>
</html>
