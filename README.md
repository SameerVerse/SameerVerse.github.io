<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>BTE Result Link Generator</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <style>
        * {
            box-sizing: border-box;
            font-family: "Segoe UI", sans-serif;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(135deg, #6a85b6, #bac8e0);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .container {
            background: #fff;
            padding: 30px;
            width: 100%;
            max-width: 420px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
        }

        h2 {
            text-align: center;
            margin-bottom: 20px;
            color: #333;
        }

        .input-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 6px;
            font-weight: 600;
            color: #555;
        }

        input {
            width: 100%;
            padding: 12px;
            border-radius: 6px;
            border: 1px solid #e0e0e0;
            font-size: 15px;
            transition: 0.3s;
        }

        input:focus {
            outline: none;
            border-color: #86a8e7;
        }

        .error {
            display: none;
            color: #e53935;
            font-size: 13px;
            margin-top: 5px;
        }

        .error.active {
            display: block;
        }

        button {
            width: 100%;
            padding: 12px;
            background: #6a85b6;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            cursor: pointer;
            transition: 0.3s;
            position: relative;
        }

        button:hover {
            background: #5a74a5;
        }

        button.loading {
            pointer-events: none;
            opacity: 0.7;
        }

        .success {
            text-align: center;
            color: #2e7d32;
            margin-top: 15px;
            display: none;
            font-weight: 600;
        }

        .success.active {
            display: block;
        }

        .result-btn {
            margin-top: 15px;
            display: none;
            background: #2e7d32;
        }

        .result-btn.active {
            display: block;
        }

        .credit {
            text-align: center;
            margin-top: 20px;
            font-size: 13px;
            color: #777;
        }

        .credit span {
            font-weight: 600;
            color: #444;
        }
    </style>
</head>

<body>

<div class="container">
    <h2>BTE Result Link Generator</h2>

    <div class="input-group">
        <label>Enrollment Number</label>
        <input type="text" id="enroll" placeholder="Enter Enrollment No">
        <div id="enroll-error" class="error">Please enter enrollment number</div>
    </div>

    <div class="input-group">
        <label>Date of Birth (DD/MM/YYYY)</label>
        <input type="text" id="dob" placeholder="DD/MM/YYYY">
        <div id="dob-error" class="error">Enter valid DOB format</div>
    </div>

    <button id="submit-btn" onclick="generateLink()">Generate Result Link</button>

    <div id="success-message" class="success">
        ✅ Link generated successfully
    </div>

    <button id="result-btn" class="result-btn" onclick="openResult()">
        View Result
    </button>

    <div class="credit">
        Developed by <span>Sameer Ansari</span>
    </div>
</div>

<script>
let generatedLink = "";

function safeBtoa(str) {
    return btoa(unescape(encodeURIComponent(str)));
}

function generateLink() {
    let enroll = document.getElementById("enroll").value.trim();
    let dob = document.getElementById("dob").value.trim();

    let submitBtn = document.getElementById("submit-btn");
    let resultBtn = document.getElementById("result-btn");
    let successMessage = document.getElementById("success-message");

    document.getElementById("enroll-error").classList.remove("active");
    document.getElementById("dob-error").classList.remove("active");

    let isValid = true;

    if (!enroll) {
        document.getElementById("enroll-error").classList.add("active");
        isValid = false;
    }

    const dobRegex = /^\d{2}\/\d{2}\/\d{4}$/;
    if (!dob || !dobRegex.test(dob)) {
        document.getElementById("dob-error").classList.add("active");
        isValid = false;
    }

    if (!isValid) {
        resultBtn.classList.remove("active");
        successMessage.classList.remove("active");
        return;
    }

    submitBtn.classList.add("loading");
    submitBtn.disabled = true;

    setTimeout(() => {
        let encEnroll = encodeURIComponent(safeBtoa(enroll));
        let encDob = encodeURIComponent(safeBtoa(dob));

        generatedLink =
            "https://result.bteexam.com/Odd_Semester/main/result.aspx?id=" +
            encEnroll +
            "&id2=" +
            encDob;

        successMessage.classList.add("active");
        resultBtn.classList.add("active");

        submitBtn.classList.remove("loading");
        submitBtn.disabled = false;
    }, 1000);
}

function openResult() {
    if (generatedLink) {
        window.open(generatedLink, "_blank");
    }
}

document.addEventListener("keydown", function(e) {
    if (e.key === "Enter") {
        generateLink();
    }
});

["enroll", "dob"].forEach(id => {
    document.getElementById(id).addEventListener("input", () => {
        document.getElementById("result-btn").classList.remove("active");
        document.getElementById("success-message").classList.remove("active");
    });
});
</script>

</body>
</html>
