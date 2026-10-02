<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>UPI Payment Link</title>
  <!-- QR Code generate karne ke liye library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    body {
      background-color: #f3f4f6;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }
    .payment-card {
      background: #ffffff;
      padding: 30px 25px;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
      text-align: center;
      max-width: 380px;
      width: 100%;
    }
    .merchant-name {
      font-size: 20px;
      font-weight: 700;
      color: #1f2937;
      margin-bottom: 5px;
    }
    .upi-id {
      font-size: 13px;
      color: #6b7280;
      margin-bottom: 20px;
    }
    .amount-box {
      font-size: 32px;
      font-weight: 800;
      color: #111827;
      margin: 15px 0;
    }
    #qrcode {
      display: flex;
      justify-content: center;
      margin: 20px 0;
      padding: 15px;
      background: #f9fafb;
      border-radius: 12px;
      border: 1px solid #e5e7eb;
    }
    .pay-btn {
      display: inline-block;
      width: 100%;
      background-color: #2563eb;
      color: #ffffff;
      text-decoration: none;
      padding: 14px;
      border-radius: 10px;
      font-weight: 600;
      font-size: 16px;
      margin-top: 10px;
      transition: background 0.2s;
    }
    .pay-btn:hover {
      background-color: #1d4ed8;
    }
    .note {
      font-size: 12px;
      color: #9ca3af;
      margin-top: 15px;
    }
  </style>
</head>
<body>

  <div class="payment-card">
    <div class="merchant-name" id="displayCustomer">Payment Request</div>
    <div class="upi-id" id="displayUpi">UPI ID</div>
    
    <div class="amount-box" id="displayAmount">₹0</div>

    <!-- QR Code Yahan Render Hoga -->
    <div id="qrcode"></div>

    <!-- Mobile par direct UPI app kholne ke liye button -->
    <a href="#" id="upiButton" class="pay-btn">Pay via UPI App</a>

    <div class="note">Scan QR with any UPI App (GPay, PhonePe, Paytm)</div>
  </div>

  <script>
    // 1. URL se parameters nikalna (?customer=...&amount=...&upi=...)
    const urlParams = new URLSearchParams(window.location.search);
    const customer = urlParams.get('rupee mountain') || 'Merchant';
    const amount = urlParams.get('14500') || '0';
    const upi = urlParams.get('8817G641I4@mairtel') || '';

    // 2. Page par text update karna
    document.getElementById('displayCustomer').innerText = customer;
    document.getElementById('displayAmount').innerText = '₹' + amount;
    document.getElementById('displayUpi').innerText = upi ? UPI: ${upi} : 'No UPI ID provided in URL';

    // 3. Standard UPI Intent Link banana
    if (upi && amount > 0) {
      const upiLink = upi://pay?pa=${encodeURIComponent(upi)}&pn=${encodeURIComponent(customer)}&am=${encodeURIComponent(amount)}&cu=INR;

      // Pay button mein link lagana
      document.getElementById('upiButton').href = upiLink;

      // QR Code banana
      new QRCode(document.getElementById("qrcode"), {
        text: upiLink,
        width: 180,
        height: 180
      });
    } else {
      document.getElementById('qrcode').innerText = "Invalid payment link parameters.";
      document.getElementById('upiButton').style.display = "none";
    }
  </script>

</body>
</html>
