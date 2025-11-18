<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rexine Cost Calculator</title>
<style>
    body { font-family: Arial, sans-serif; margin: 20px; padding: 0; background: #f5f5f5; }
    .card { background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
    label { font-size: 14px; font-weight: bold; display: block; margin-top: 12px; }
    input { width: 100%; padding: 12px; margin-top: 6px; border: 1px solid #ccc; border-radius: 8px; font-size: 16px; }
    button { width: 100%; padding: 14px; margin-top: 18px; border: none; border-radius: 10px; font-size: 16px; font-weight: bold; cursor: pointer; background: #222; color: #fff; }
    #result { margin-top: 20px; font-size: 18px; font-weight: bold; }
</style>
</head>
<body>

<div class="card">
<h2>Rexine Piece Cost Calculator</h2>

<label>Sheet Width (inches)</label>
<input type="number" id="sheetWidth" value="54">

<label>Sheet Height (meters)</label>
<input type="number" id="sheetHeight" value="0.5" step="0.01">

<label>Piece Width (inches)</label>
<input type="number" id="pieceW" value="4.25" step="0.01">

<label>Piece Height (inches)</label>
<input type="number" id="pieceH" value="4.25" step="0.01">

<label>Total Sheet Cost (₹)</label>
<input type="number" id="sheetCost" value="160">

<button onclick="calc()">Calculate</button>

<div id="result"></div>
</div>

<script>
function calc() {
    let sheetWidth = parseFloat(document.getElementById('sheetWidth').value);
    let sheetHeightMeters = parseFloat(document.getElementById('sheetHeight').value);
    let pieceW = parseFloat(document.getElementById('pieceW').value);
    let pieceH = parseFloat(document.getElementById('pieceH').value);
    let sheetCost = parseFloat(document.getElementById('sheetCost').value);

    // convert meters to inches
    let sheetHeight = sheetHeightMeters * 39.3701;

    // number of pieces that actually fit
    let piecesAcross = Math.floor(sheetWidth / pieceW);
    let piecesDown = Math.floor(sheetHeight / pieceH);
    let totalPieces = piecesAcross * piecesDown;

    let costPerPiece = sheetCost / totalPieces;

    document.getElementById("result").innerHTML =
        "Total usable pieces: " + totalPieces +
        "<br>Cost per piece: ₹" + costPerPiece.toFixed(2);
}
</script>

</body>
</html>
