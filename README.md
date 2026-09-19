# Activity No. 1: Number System Converter and Arithmetic Calculator
**Name:** Wilfred Justin D. Peteros  
**Course & Section:** CPE463-H2  
**Instructor:** Engr. Johnalyn L. Figueras

### System Requirements
1. Hardware: A computer or laptop.
2. Software: A modern web browser like Google Chrome or Microsoft Edge.
3. Network: An active internet connection to load the Tailwind CSS framework via CDN and Google Fonts.
4. Tools: A basic text editor such as Notepad for code modification.

### Algorithm
1. Start the program.
2. Initialize the user interface with three default input cases, a custom arithmetic expression field, and a dedicated complement subtraction tool.
3. Dynamically assign a sequential alphabetical variable (A, B, C...) to each input case.
4. Prompt the user to enter a number and select its corresponding base (Binary, Octal, Decimal, Hexadecimal) for each active input.
5. Validate each input string against the allowed characters for its selected base, including support for fractional floating-point values.
6. Calculate the standard conversions (Binary, Octal, Decimal, Hexadecimal).
7. Calculate the 1's Complement and 2's Complement for the integer part of each valid input using dynamic bit-padding (8-bit, 16-bit, 32-bit depending on magnitude).
8. Display all six representations (Binary, Octal, Decimal, Hexadecimal, 1's Comp, 2's Comp) in the individual results grid.
9. Convert all valid input strings into a standard decimal floating-point format using a custom fractional parser and map them to their assigned alphabetical variables.
10. Read the custom arithmetic expression input by the user. Parse the expression using the Shunting-yard algorithm to evaluate strict operator precedence and parenthetical grouping logic.
11. Display the formatted equation breakdown and final computed arithmetic result in all base systems and complement formats.
12. Populate the Complement Subtraction Tool with valid variables. When two variables are selected (Minuend and Subtrahend), compute and display the step-by-step binary subtraction process utilizing both the 1's Complement Method (End-Around Carry) and 2's Complement Method.
13. Automatically recalculate and reassign variables (preventing alphabetical gaps) if the user dynamically adds or removes input cases.
14. Handle and display explicit errors for invalid inputs, syntax errors, mismatched parentheses, or mathematical impossibilities (division by zero).
15. End the program.

### Pseudocode
```text
START PROGRAM
  SET caseCounter = 0
  CALL addCase() THREE TIMES to render initial interface

  FUNCTION getComplements(num)
    SET intVal = TRUNCATE(num)
    SET bits = CALCULATE required bits (8, 16, 32) based on intVal
    SET onesComp = INVERT BITS OF (absolute intVal in binary)
    SET twosComp = ADD 1 TO onesComp
    RETURN onesComp, twosComp
  END FUNCTION

  FUNCTION calculateTotal()
    READ exprString FROM math-expression input
    SET variables = EMPTY MAP
    
    FOR EACH active case container:
      READ rawValue AND inBase
      IF rawValue IS INVALID THEN ABORT AND DISPLAY ERROR
      
      SET decValue = CUSTOM PARSE rawValue TO Decimal FLOAT
      STORE decValue IN variables[assignedLetter]
      
      DISPLAY Base Conversions AND getComplements(decValue)
    END FOR
    
    POPULATE Complement Subtraction Selectors WITH keys OF variables

    TRY
      SET postfix = SHUNTING_YARD_PARSE(exprString)
      SET finalResult = EVALUATE_POSTFIX(postfix, variables)
      
      DISPLAY finalResult IN Base 2, 8, 10, 16, 1's Comp, 2's Comp
    CATCH ERROR
      DISPLAY explicit error message
    END TRY
  END FUNCTION

  FUNCTION calcComplementSub()
    READ minuendVar AND subtrahendVar
    SET M = TRUNCATE(variables[minuendVar])
    SET S = ABS(TRUNCATE(variables[subtrahendVar]))
    
    SET paddedM = BINARY(M) padded to common bit length
    SET paddedS = BINARY(S) padded to common bit length
    
    // 1's Complement Method
    SET onesS = INVERT(paddedS)
    SET sum1 = paddedM + onesS
    IF carry THEN sum1 = sum1 + 1 (End Around Carry)
    
    // 2's Complement Method
    SET twosS = INVERT(paddedS) + 1
    SET sum2 = paddedM + twosS (Discard Carry)
    
    DISPLAY step-by-step calculations FOR both methods
  END FUNCTION
END PROGRAM
```

### Flowchart
```mermaid
graph TD
  Start((User Types Number or Formula)) --> Indiv[Process Each Input Box]

  subgraph Individual Number Conversion
    Indiv --> IsEmpty{Is the box empty?}
    IsEmpty -- Yes --> Await[Display 'Awaiting input...']
    IsEmpty -- No --> ValidChars{Are characters valid for selected base?}
    ValidChars -- No --> ErrChars[Display 'Invalid input' Error]
    ValidChars -- Yes --> ConvertNum[Convert to standard decimal value]
    ConvertNum --> FormatIndiv[Display in Base 2, 8, 10, 16]
    ConvertNum --> CompIndiv[Calculate & Display 1's and 2's Complement]
  end

  FormatIndiv --> CalcTotal[Start Final Math Calculation]
  CompIndiv --> CalcTotal
  Await --> CalcTotal
  ErrChars --> CalcTotal

  subgraph Final Equation Processing
    CalcTotal --> CheckAll{Are all numbers valid?}
    CheckAll -- No --> WarnInputs[Display 'Enter valid numbers' warning]
    CheckAll -- Yes --> ReadExpr[Check Arithmetic Expression Box]

    ReadExpr --> IsExprEmpty{Is formula missing?}
    IsExprEmpty -- Yes --> WarnExpr[Display 'Please enter expression']
    IsExprEmpty -- No --> ReadSymbols[Break formula into letters and symbols]

    ReadSymbols --> ValidTokens{Are the symbols allowed?}
    ValidTokens -- No --> ErrToken[Display 'Invalid characters' Error]
    ValidTokens -- Yes --> MathRules[Apply Math Rules / PEMDAS]

    MathRules --> TryEval{Are parentheses matched & formula logical?}
    TryEval -- No --> ErrSyntax[Display 'Syntax Error']
    TryEval -- Yes --> Eval[Calculate the Answer]

    Eval --> DivZero{Is it dividing by zero?}
    DivZero -- Yes --> ErrDiv[Display 'Division by zero' Error]
    DivZero -- No --> FinalConvert[Convert Final Answer to all bases & complements]
    FinalConvert --> UpdateCompSub[Update Complement Subtraction Dropdowns]
    FinalConvert --> Output[Show Equation Breakdown & Final Results Grid]
  end

  subgraph Complement Subtraction Tool
    UpdateCompSub --> UserSelect[User Selects Minuend & Subtrahend]
    UserSelect --> PadBits[Pad Binary to 8/16/32 Bits]
    PadBits --> Math1[Execute 1's Complement Math & End-Around Carry]
    PadBits --> Math2[Execute 2's Complement Math]
    Math1 --> ShowSteps[Display Step-by-Step UI Breakdown]
    Math2 --> ShowSteps
  end
```

### Program Implementation
```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Number System Converter & Calculator</title>
  
  <link rel="preconnect" href="[https://fonts.googleapis.com](https://fonts.googleapis.com)">
  <link rel="preconnect" href="[https://fonts.gstatic.com](https://fonts.gstatic.com)" crossorigin>
  <link href="[https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&family=Space+Grotesk:wght@500;700&display=swap](https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&family=Space+Grotesk:wght@500;700&display=swap)" rel="stylesheet">
  
  <script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['"Plus Jakarta Sans"', 'sans-serif'],
            mono: ['"Space Grotesk"', 'monospace'],
          }
        }
      }
    }
  </script>
</head>
<body class="bg-slate-50 dark:bg-slate-950 min-h-screen p-4 md:p-8 font-sans text-slate-800 dark:text-indigo-100 transition-colors duration-300">

  <div class="max-w-6xl mx-auto">
    <div class="flex justify-end mb-4">
      <button onclick="toggleTheme()" class="p-3 rounded-full shadow-lg bg-indigo-100 dark:bg-slate-800 text-indigo-700 dark:text-purple-400 hover:bg-indigo-200 dark:hover:bg-slate-700 border border-indigo-200 dark:border-indigo-800 transition-colors focus:outline-none focus:ring-2 focus:ring-indigo-500" title="Toggle Light/Dark Mode">
        <svg class="w-6 h-6 hidden dark:block" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path>
        </svg>
        <svg class="w-6 h-6 block dark:hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path>
        </svg>
      </button>
    </div>

    <div class="bg-white dark:bg-slate-900 p-6 md:p-8 rounded-2xl shadow-xl border border-slate-200 dark:border-indigo-900 transition-colors duration-300">
      <h1 class="text-3xl md:text-4xl font-extrabold mb-8 text-center text-indigo-700 dark:text-purple-400 font-mono tracking-tight">System Converter & Calculator</h1>
      
      <div class="mb-8 p-6 bg-indigo-50 dark:bg-indigo-950/30 border border-indigo-200 dark:border-indigo-800 rounded-xl">
        <label class="block text-lg font-bold text-indigo-800 dark:text-purple-300 mb-3 text-center">Arithmetic Expression</label>
        <p class="text-sm text-center text-indigo-600 dark:text-indigo-400 mb-4">Use variables (A, B, C...) and operators (+, -, *, /, parentheses). Example: <strong>(A + B - C) * D</strong></p>
        <input type="text" id="math-expression" class="w-full md:w-3/4 mx-auto block bg-white dark:bg-slate-900 border border-indigo-300 dark:border-indigo-700 p-4 rounded-lg text-2xl text-slate-900 dark:text-white font-bold text-center focus:outline-none focus:ring-4 focus:ring-indigo-500 dark:focus:ring-purple-500 transition-colors shadow-sm uppercase tracking-widest" placeholder="A + B - C" value="A + B - C" oninput="calculateTotal()">
      </div>

      <div id="cases-container" class="grid grid-cols-1 gap-6">
        <!-- Dynamic cases injected here -->
      </div>

      <div class="mt-6 text-center">
        <button onclick="addCase()" class="bg-indigo-600 hover:bg-indigo-700 dark:bg-slate-800 dark:hover:bg-slate-700 border border-transparent dark:border-indigo-700 text-white dark:text-purple-300 font-bold py-3 px-8 rounded-xl transition-colors shadow-lg focus:outline-none focus:ring-2 focus:ring-indigo-500">
          + Add Input Number
        </button>
      </div>

      <div class="mt-12 p-8 bg-slate-900 dark:bg-black border-2 border-indigo-500 dark:border-purple-600 rounded-2xl shadow-2xl relative overflow-hidden">
        <div class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500"></div>
        <h2 class="text-2xl font-bold text-white mb-6 text-center">Final Arithmetic Result</h2>
        <div id="final-total-container" class="text-center">
          <p class="text-slate-400 text-lg">Enter valid numbers and a proper expression above to compute the result.</p>
        </div>
      </div>

      <div class="mt-8 p-8 bg-slate-800 dark:bg-slate-950 border-2 border-emerald-500 dark:border-emerald-600 rounded-2xl shadow-xl relative overflow-hidden">
        <div class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-emerald-400 to-teal-500"></div>
        <h2 class="text-2xl font-bold text-white mb-2 text-center">Complement Subtraction Method</h2>
        <p class="text-sm text-center text-emerald-400 mb-6">Select two variables to demonstrate step-by-step subtraction (Minuend - Subtrahend) using complements. Uses absolute integer parts.</p>
        
        <div class="flex justify-center items-center gap-4 mb-6">
          <select id="comp-minuend" class="bg-slate-700 text-white p-3 rounded-lg border border-slate-600 focus:ring-2 focus:ring-emerald-500 font-bold text-lg" onchange="calcComplementSub()"></select>
          <span class="text-white text-3xl font-bold">-</span>
          <select id="comp-subtrahend" class="bg-slate-700 text-white p-3 rounded-lg border border-slate-600 focus:ring-2 focus:ring-emerald-500 font-bold text-lg" onchange="calcComplementSub()"></select>
        </div>

        <div id="comp-sub-output" class="text-slate-300 text-center font-mono">
           Awaiting variable selection...
        </div>
      </div>

    </div>
  </div>

  <script>
    let caseCounter = 0;
    window.globalVars = {};

    function toggleTheme() {
      document.documentElement.classList.toggle('dark');
    }

    function parseToDecimal(str, base) {
      const isNegative = str.startsWith('-');
      const cleanStr = str.replace('-', '');
      const parts = cleanStr.split('.');

      let intPart = parseInt(parts[0] || '0', base);
      let fracPart = 0;

      if (parts.length > 1) {
        let fracStr = parts[1];
        let divisor = base;
        for (let i = 0; i < fracStr.length; i++) {
          fracPart += parseInt(fracStr[i], base) / divisor;
          divisor *= base;
        }
      }

      let total = intPart + fracPart;
      return isNegative ? -total : total;
    }

    function formatBase(num, base) {
      if (isNaN(num)) return "NaN";
      const isNeg = num < 0;
      const absVal = Math.abs(num);
      let str = absVal.toString(base).toUpperCase();
      return (isNeg ? '-' : '') + str;
    }

    function getComplements(num) {
      if (isNaN(num)) return { ones: "NaN", twos: "NaN" };
      let intVal = Math.trunc(Math.abs(num)); 
      let binStr = intVal.toString(2);
      
      let bits = 8;
      while (binStr.length > bits - 1) bits += 8; 
      binStr = binStr.padStart(bits, '0');

      let onesComp = binStr.split('').map(b => b === '0' ? '1' : '0').join('');
      
      let twosComp = '';
      let carry = 1;
      for (let i = bits - 1; i >= 0; i--) {
        let sum = parseInt(onesComp[i]) + carry;
        twosComp = (sum % 2) + twosComp;
        carry = Math.floor(sum / 2);
      }
      
      return { ones: onesComp, twos: twosComp, bits: bits };
    }

    function addBinaryStr(a, b) {
        let res = '', carry = 0;
        for(let i = a.length - 1; i >= 0; i--) {
            let sum = parseInt(a[i]) + parseInt(b[i]) + carry;
            res = (sum % 2) + res;
            carry = Math.floor(sum / 2);
        }
        return { sum: res, carry: carry };
    }

    function createCaseHTML(id) {
      return `
        <div id="case-${id}" data-var="" class="p-6 border border-slate-200 dark:border-indigo-800 rounded-xl bg-slate-50 dark:bg-slate-800 relative shadow-sm dark:shadow-inner transition-colors duration-300">
          <div class="flex justify-between items-center mb-4">
            <label class="case-label font-bold text-xl text-indigo-800 dark:text-purple-300 bg-indigo-100 dark:bg-indigo-900/50 px-4 py-1 rounded-full border border-indigo-200 dark:border-indigo-700">Input</label>
            <button onclick="removeCase(${id})" class="remove-btn p-2 text-red-500 dark:text-red-400 hover:text-red-700 dark:hover:text-red-300 hover:bg-red-100 dark:hover:bg-red-900/30 rounded-lg hidden transition-colors focus:outline-none" title="Remove Input">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
              </svg>
            </button>
          </div>
          
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
            <div>
              <label class="block text-sm font-semibold text-slate-600 dark:text-indigo-300 mb-1">Value</label>
              <input type="text" id="val-${id}" class="w-full bg-white dark:bg-slate-900 border border-slate-300 dark:border-indigo-700 p-3 rounded-lg text-lg font-mono text-slate-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-indigo-500 dark:focus:ring-purple-500 placeholder-slate-400 dark:placeholder-slate-500 transition-colors" placeholder="0" oninput="handleInput(${id})">
            </div>
            <div>
              <label class="block text-sm font-semibold text-slate-600 dark:text-indigo-300 mb-1">Base System</label>
              <select id="inBase-${id}" class="w-full bg-white dark:bg-slate-900 border border-slate-300 dark:border-indigo-700 p-3 rounded-lg text-lg text-slate-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-indigo-500 dark:focus:ring-purple-500 transition-colors" onchange="handleInput(${id})">
                <option value="2">Binary (Base 2)</option>
                <option value="8">Octal (Base 8)</option>
                <option value="10" selected>Decimal (Base 10)</option>
                <option value="16">Hexadecimal (Base 16)</option>
              </select>
            </div>
          </div>

          <div id="out-${id}" class="mt-4 p-4 bg-white dark:bg-slate-950 border border-slate-200 dark:border-indigo-900 rounded-lg min-h-[80px] flex items-center justify-center text-slate-500 dark:text-indigo-400 transition-colors duration-300">
            Awaiting input...
          </div>
        </div>
      `;
    }

    function reindexLabels() {
      const cases = document.querySelectorAll('[id^="case-"]');
      cases.forEach((caseEl, index) => {
        const varLetter = String.fromCharCode(65 + index); 
        caseEl.setAttribute('data-var', varLetter);
        const labelEl = caseEl.querySelector('.case-label');
        if (labelEl) {
          labelEl.textContent = `Input ${varLetter}`;
        }
      });
    }

    function addCase() {
      caseCounter++;
      const container = document.getElementById('cases-container');
      container.insertAdjacentHTML('beforeend', createCaseHTML(caseCounter));
      reindexLabels();
      updateRemoveButtons();
      calculateTotal();
    }

    function removeCase(id) {
      const caseEl = document.getElementById(`case-${id}`);
      if (caseEl && document.querySelectorAll('[id^="case-"]').length > 2) {
        caseEl.remove();
        reindexLabels();
        updateRemoveButtons();
        calculateTotal();
      }
    }

    function updateRemoveButtons() {
      const buttons = document.querySelectorAll('.remove-btn');
      if (buttons.length <= 2) {
        buttons.forEach(btn => btn.classList.add('hidden'));
      } else {
        buttons.forEach(btn => btn.classList.remove('hidden'));
      }
    }

    function handleInput(id) {
      convertIndividualNumber(id);
      calculateTotal();
    }

    function convertIndividualNumber(id) {
      const rawValue = document.getElementById(`val-${id}`).value.trim();
      const inBase = parseInt(document.getElementById(`inBase-${id}`).value);
      const outputDiv = document.getElementById(`out-${id}`);

      if (rawValue === "") {
        outputDiv.innerHTML = "Awaiting input...";
        outputDiv.className = "mt-4 p-4 bg-white dark:bg-slate-950 border border-slate-200 dark:border-indigo-900 rounded-lg min-h-[80px] flex items-center justify-center text-slate-500 dark:text-indigo-400 text-lg transition-colors";
        return;
      }

      let isValid = false;
      if (inBase === 2) isValid = /^-?[01]+(\.[01]+)?$/.test(rawValue);
      if (inBase === 8) isValid = /^-?[0-7]+(\.[0-7]+)?$/.test(rawValue);
      if (inBase === 10) isValid = /^-?[0-9]+(\.[0-9]+)?$/.test(rawValue);
      if (inBase === 16) isValid = /^-?[0-9a-fA-F]+(\.[0-9a-fA-F]+)?$/.test(rawValue);

      if (!isValid) {
        outputDiv.innerHTML = "<span class='text-red-600 dark:text-red-400 font-bold'>Invalid input for selected base.</span>";
        outputDiv.className = "mt-4 p-4 bg-white dark:bg-slate-950 border border-slate-200 dark:border-indigo-900 rounded-lg min-h-[80px] flex items-center justify-center transition-colors";
        return;
      }

      let decValue = parseToDecimal(rawValue, inBase);
      let comps = getComplements(decValue);

      const binStr = formatBase(decValue, 2);
      const octStr = formatBase(decValue, 8);
      const decStr = formatBase(decValue, 10);
      const hexStr = formatBase(decValue, 16);

      outputDiv.innerHTML = `
        <div class="grid grid-cols-2 lg:grid-cols-3 xl:grid-cols-6 gap-3 w-full">
          <div class="bg-slate-50 dark:bg-slate-900 p-3 rounded-lg border border-slate-200 dark:border-indigo-800 min-w-0 flex flex-col">
            <span class="text-xs font-semibold text-slate-500 dark:text-indigo-400 mb-1">Binary</span>
            <div class="overflow-x-auto"><span class="text-sm font-mono font-bold text-indigo-700 dark:text-purple-300 break-all">${binStr}</span></div>
          </div>
          <div class="bg-slate-50 dark:bg-slate-900 p-3 rounded-lg border border-slate-200 dark:border-indigo-800 min-w-0 flex flex-col">
            <span class="text-xs font-semibold text-slate-500 dark:text-indigo-400 mb-1">Octal</span>
            <div class="overflow-x-auto"><span class="text-sm font-mono font-bold text-indigo-700 dark:text-purple-300 break-all">${octStr}</span></div>
          </div>
          <div class="bg-slate-50 dark:bg-slate-900 p-3 rounded-lg border border-slate-200 dark:border-indigo-800 min-w-0 flex flex-col">
            <span class="text-xs font-semibold text-slate-500 dark:text-indigo-400 mb-1">Decimal</span>
            <div class="overflow-x-auto"><span class="text-sm font-mono font-bold text-indigo-700 dark:text-purple-300 break-all">${decStr}</span></div>
          </div>
          <div class="bg-slate-50 dark:bg-slate-900 p-3 rounded-lg border border-slate-200 dark:border-indigo-800 min-w-0 flex flex-col">
            <span class="text-xs font-semibold text-slate-500 dark:text-indigo-400 mb-1">Hexadecimal</span>
            <div class="overflow-x-auto"><span class="text-sm font-mono font-bold text-indigo-700 dark:text-purple-300 break-all">${hexStr}</span></div>
          </div>
          <div class="bg-indigo-50 dark:bg-indigo-950/40 p-3 rounded-lg border border-indigo-200 dark:border-indigo-800 min-w-0 flex flex-col">
            <span class="text-xs font-semibold text-indigo-600 dark:text-indigo-400 mb-1">1's Comp (Int)</span>
            <div class="overflow-x-auto"><span class="text-sm font-mono font-bold text-indigo-800 dark:text-purple-200 break-all">${comps.ones}</span></div>
          </div>
          <div class="bg-indigo-50 dark:bg-indigo-950/40 p-3 rounded-lg border border-indigo-200 dark:border-indigo-800 min-w-0 flex flex-col">
            <span class="text-xs font-semibold text-indigo-600 dark:text-indigo-400 mb-1">2's Comp (Int)</span>
            <div class="overflow-x-auto"><span class="text-sm font-mono font-bold text-indigo-800 dark:text-purple-200 break-all">${comps.twos}</span></div>
          </div>
        </div>
      `;
      outputDiv.className = "mt-4 p-3 bg-white dark:bg-slate-950 border border-slate-200 dark:border-indigo-900 rounded-lg min-h-[80px] transition-colors";
    }

    function evaluatePostfix(postfix, variables) {
      const valStack = [];
      for (let token of postfix) {
        if (typeof token === 'number') {
          valStack.push(token);
        } else if (variables.hasOwnProperty(token)) {
          valStack.push(variables[token]);
        } else {
          if (valStack.length < 2) throw new Error("Invalid expression syntax.");
          const b = valStack.pop();
          const a = valStack.pop();
          if (token === '+') valStack.push(a + b);
          else if (token === '-') valStack.push(a - b);
          else if (token === '*') valStack.push(a * b);
          else if (token === '/') {
            if (b === 0) throw new Error("Division by zero error.");
            valStack.push(a / b);
          }
        }
      }
      if (valStack.length !== 1) throw new Error("Invalid expression syntax.");
      return valStack[0];
    }

    function calculateTotal() {
      const cases = document.querySelectorAll('[id^="case-"]');
      const totalDiv = document.getElementById('final-total-container');
      const exprString = document.getElementById('math-expression').value.trim().toUpperCase();
      
      window.globalVars = {};
      let displayStrs = {};
      let allValid = true;

      cases.forEach(caseEl => {
        const id = caseEl.id.split('-')[1];
        const varName = caseEl.getAttribute('data-var');
        const rawValue = document.getElementById(`val-${id}`).value.trim();
        const inBase = parseInt(document.getElementById(`inBase-${id}`).value);

        if (!rawValue) {
          allValid = false;
          return;
        }

        let isValid = false;
        if (inBase === 2) isValid = /^-?[01]+(\.[01]+)?$/.test(rawValue);
        if (inBase === 8) isValid = /^-?[0-7]+(\.[0-7]+)?$/.test(rawValue);
        if (inBase === 10) isValid = /^-?[0-9]+(\.[0-9]+)?$/.test(rawValue);
        if (inBase === 16) isValid = /^-?[0-9a-fA-F]+(\.[0-9a-fA-F]+)?$/.test(rawValue);

        if (!isValid) {
          allValid = false;
          return;
        }

        let decValue = parseToDecimal(rawValue, inBase);
        window.globalVars[varName] = decValue;

        const sub = inBase === 2 ? '₂' : inBase === 8 ? '₈' : inBase === 10 ? '₁₀' : '₁₆';
        displayStrs[varName] = `(${rawValue})${sub}`;
      });

      updateComplementSelects();

      if (!allValid || Object.keys(window.globalVars).length === 0) {
        totalDiv.innerHTML = "<p class='text-slate-400 text-lg'>Enter valid numbers in all active fields to compute.</p>";
        return;
      }
      
      if (!exprString) {
        totalDiv.innerHTML = "<p class='text-slate-400 text-lg'>Please enter a mathematical expression.</p>";
        return;
      }

      const tokens = exprString.match(/[A-Z]+|[0-9]*\.?[0-9]+|[+\-*/()]/g);
      if (!tokens) {
        totalDiv.innerHTML = "<p class='text-red-400 text-xl font-bold'>Error: Invalid characters in expression.</p>";
        return;
      }

      const precedence = { '+': 1, '-': 1, '*': 2, '/': 2 };
      const postfix = [];
      const opStack = [];
      let formattedEqTokens = [];

      try {
        for (let token of tokens) {
          if (window.globalVars.hasOwnProperty(token)) {
            postfix.push(token);
            formattedEqTokens.push(`<span class="text-white">${displayStrs[token]}</span>`);
          } else if (/^[0-9]*\.?[0-9]+$/.test(token)) {
            postfix.push(Number(token));
            formattedEqTokens.push(`<span class="text-white">${token}</span>`);
          } else if (['+', '-', '*', '/'].includes(token)) {
            let symbol = token === '*' ? '×' : token === '/' ? '÷' : token;
            formattedEqTokens.push(`<span class="text-pink-400 mx-1">${symbol}</span>`);
            while (opStack.length > 0 && opStack[opStack.length - 1] !== '(' &&
                   precedence[opStack[opStack.length - 1]] >= precedence[token]) {
              postfix.push(opStack.pop());
            }
            opStack.push(token);
          } else if (token === '(') {
            formattedEqTokens.push(`<span class="text-indigo-400">(</span>`);
            opStack.push(token);
          } else if (token === ')') {
            formattedEqTokens.push(`<span class="text-indigo-400">)</span>`);
            while (opStack.length > 0 && opStack[opStack.length - 1] !== '(') {
              postfix.push(opStack.pop());
            }
            if (opStack.length === 0) throw new Error("Mismatched parentheses.");
            opStack.pop(); 
          } else {
            throw new Error(`Unknown variable: ${token}`);
          }
        }
        while (opStack.length > 0) {
          const op = opStack.pop();
          if (op === '(' || op === ')') throw new Error("Mismatched parentheses.");
          postfix.push(op);
        }

        const finalResult = evaluatePostfix(postfix, window.globalVars);
        const comps = getComplements(finalResult);

        const binStr = formatBase(finalResult, 2);
        const octStr = formatBase(finalResult, 8);
        const decStr = formatBase(finalResult, 10);
        const hexStr = formatBase(finalResult, 16);

        totalDiv.innerHTML = `
          <div class="mb-6 p-4 bg-slate-800 rounded-lg border border-slate-700 shadow-inner overflow-x-auto">
            <p class="text-indigo-300 text-sm font-bold uppercase tracking-wider mb-2">Equation Breakdown</p>
            <p class="text-xl md:text-2xl font-mono whitespace-nowrap">${formattedEqTokens.join('')} <span class="text-pink-400 mx-2">=</span></p>
          </div>
          
          <div class="grid grid-cols-2 lg:grid-cols-3 xl:grid-cols-6 gap-4">
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 text-left overflow-hidden">
              <span class="block text-sm font-bold text-slate-400 mb-1">Binary</span>
              <div class="overflow-x-auto"><span class="text-xl md:text-2xl font-mono font-bold text-green-400 break-all">${binStr}</span></div>
            </div>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 text-left overflow-hidden">
              <span class="block text-sm font-bold text-slate-400 mb-1">Octal</span>
              <div class="overflow-x-auto"><span class="text-xl md:text-2xl font-mono font-bold text-blue-400 break-all">${octStr}</span></div>
            </div>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 text-left overflow-hidden">
              <span class="block text-sm font-bold text-slate-400 mb-1">Decimal</span>
              <div class="overflow-x-auto"><span class="text-xl md:text-2xl font-mono font-bold text-yellow-400 break-all">${decStr}</span></div>
            </div>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 text-left overflow-hidden">
              <span class="block text-sm font-bold text-slate-400 mb-1">Hexadecimal</span>
              <div class="overflow-x-auto"><span class="text-xl md:text-2xl font-mono font-bold text-pink-400 break-all">${hexStr}</span></div>
            </div>
            <div class="bg-slate-800 p-4 rounded-xl border border-indigo-500/50 text-left overflow-hidden">
              <span class="block text-sm font-bold text-indigo-300 mb-1">1's Comp (Int)</span>
              <div class="overflow-x-auto"><span class="text-xl md:text-2xl font-mono font-bold text-indigo-400 break-all">${comps.ones}</span></div>
            </div>
            <div class="bg-slate-800 p-4 rounded-xl border border-indigo-500/50 text-left overflow-hidden">
              <span class="block text-sm font-bold text-indigo-300 mb-1">2's Comp (Int)</span>
              <div class="overflow-x-auto"><span class="text-xl md:text-2xl font-mono font-bold text-indigo-400 break-all">${comps.twos}</span></div>
            </div>
          </div>
        `;
      } catch (err) {
        totalDiv.innerHTML = `<p class='text-red-400 text-xl font-bold'>Error: ${err.message}</p>`;
      }
    }

    function updateComplementSelects() {
      const minuendSel = document.getElementById('comp-minuend');
      const subtrahendSel = document.getElementById('comp-subtrahend');
      const mVal = minuendSel.value;
      const sVal = subtrahendSel.value;

      minuendSel.innerHTML = '';
      subtrahendSel.innerHTML = '';

      Object.keys(window.globalVars).forEach(key => {
        minuendSel.add(new Option(`Var ${key}`, key));
        subtrahendSel.add(new Option(`Var ${key}`, key));
      });

      if(mVal && window.globalVars[mVal] !== undefined) minuendSel.value = mVal;
      if(sVal && window.globalVars[sVal] !== undefined) subtrahendSel.value = sVal;
      else if(Object.keys(window.globalVars).length > 1) subtrahendSel.selectedIndex = 1;

      calcComplementSub();
    }

    function calcComplementSub() {
      let mKey = document.getElementById('comp-minuend').value;
      let sKey = document.getElementById('comp-subtrahend').value;
      let out = document.getElementById('comp-sub-output');

      if(!mKey || !sKey || window.globalVars[mKey] === undefined || window.globalVars[sKey] === undefined) {
        out.innerHTML = "Awaiting valid variables for calculation...";
        return;
      }

      let mVal = Math.trunc(window.globalVars[mKey]);
      let sVal = Math.abs(Math.trunc(window.globalVars[sKey])); 

      let maxAbs = Math.max(Math.abs(mVal), sVal);
      let bits = 8;
      while(maxAbs >= Math.pow(2, bits - 1)) bits += 8;

      function toSignedBin(val, bits) {
        if (val >= 0) return val.toString(2).padStart(bits, '0');
        let posBin = Math.abs(val).toString(2).padStart(bits, '0');
        let flipped = posBin.split('').map(b => b==='0'?'1':'0').join('');
        return addBinaryStr(flipped, '1'.padStart(bits, '0')).sum;
      }

      let mBin = toSignedBin(mVal, bits);
      let sBinAbs = toSignedBin(sVal, bits);
      
      let sOnes = sBinAbs.split('').map(b => b==='0'?'1':'0').join('');
      let sTwos = addBinaryStr(sOnes, '1'.padStart(bits, '0')).sum;

      let add1 = addBinaryStr(mBin, sOnes);
      let finalOnes = add1.sum;
      let endCarryTxt = "No carry, result is negative (in 1's complement form).";
      if (add1.carry) {
        finalOnes = addBinaryStr(add1.sum, '1'.padStart(bits, '0')).sum;
        endCarryTxt = "Carry = 1. Add End-Around Carry (+1).";
      }

      let add2 = addBinaryStr(mBin, sTwos);
      let finalTwos = add2.sum;

      out.innerHTML = `
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8 text-left mt-4">
          <div class="bg-slate-900 p-6 rounded-xl border border-slate-700 shadow-inner">
            <h3 class="text-teal-400 font-bold mb-4 border-b border-slate-700 pb-2">1's Complement Method</h3>
            <p class="text-slate-400 mb-1">M: <span class="text-white">${mBin}</span></p>
            <p class="text-slate-400 mb-1">S: <span class="text-white">${sBinAbs}</span></p>
            <p class="text-slate-400 mb-3 border-b border-slate-700 pb-2">1's Comp(S): <span class="text-pink-400">${sOnes}</span></p>
            <p class="text-slate-400 mb-1">Add M + 1's Comp(S):</p>
            <p class="text-white font-bold tracking-widest bg-slate-800 p-2 rounded mb-2">${mBin}<br>+${sOnes}<br>------------<br>${add1.carry ? '1 ' : '0 '}${add1.sum}</p>
            <p class="text-emerald-400 text-sm mb-2">${endCarryTxt}</p>
            <p class="text-white font-bold tracking-widest bg-slate-800 p-2 rounded">Final: ${finalOnes}</p>
          </div>
          
          <div class="bg-slate-900 p-6 rounded-xl border border-slate-700 shadow-inner">
            <h3 class="text-teal-400 font-bold mb-4 border-b border-slate-700 pb-2">2's Complement Method</h3>
            <p class="text-slate-400 mb-1">M: <span class="text-white">${mBin}</span></p>
            <p class="text-slate-400 mb-1">S: <span class="text-white">${sBinAbs}</span></p>
            <p class="text-slate-400 mb-3 border-b border-slate-700 pb-2">2's Comp(S): <span class="text-pink-400">${sTwos}</span></p>
            <p class="text-slate-400 mb-1">Add M + 2's Comp(S):</p>
            <p class="text-white font-bold tracking-widest bg-slate-800 p-2 rounded mb-2">${mBin}<br>+${sTwos}<br>------------<br>${add2.carry ? '1 ' : '0 '}${add2.sum}</p>
            <p class="text-emerald-400 text-sm mb-2">Discard carry out.</p>
            <p class="text-white font-bold tracking-widest bg-slate-800 p-2 rounded">Final: ${finalTwos}</p>
          </div>
        </div>
      `;
    }

    addCase();
    addCase();
    addCase();
  </script>

</body>
</html>
```

### Test Cases

**Test Case 1: Binary Conversion**
![Sample-1](sample-outputs/Sample-Output-1.png)
1. Input Value: 1010
2. Selected Base: Binary (Base 2)
3. Expected Output: Binary: 1010, Octal: 12, Decimal: 10, Hexadecimal: A

**Test Case 2: Hexadecimal Conversion**
![Sample-2](sample-outputs/Sample-Output-2.png)
1. Input Value: 1F
2. Selected Base: Hexadecimal (Base 16)
3. Expected Output: Binary: 11111, Octal: 37, Decimal: 31, Hexadecimal: 1F

**Test Case 3: Invalid Input Handling**
![Sample-3](sample-outputs/Sample-Output-3.png)
1. Input Value: 9
2. Selected Base: Octal (Base 8)
3. Expected Output: "Invalid input for selected base." text appears in red.

**Test Case 4: Mathematical Bounds Limits**
![Sample-4](sample-outputs/Sample-Output-4.png)
1. Input Value: 99999999999999999999
2. Selected Base: Decimal (Base 10)
3. Expected Output: Hexadecimal correctly outputs 56BC75E2D63100000 instead of converting the string into scientific notation.

**Test Case 5: Operator Precedence (Binary + Octal + Decimal)**
![Sample-5](sample-outputs/Sample-Output-5.png)
![Sample-5-1](sample-outputs/Sample-Output-5-1.png)
1. Inputs: `Input A = 1010` (Base 2), `Input B = 20` (Base 8), `Input C = 2` (Base 10)
2. Arithmetic Expression: `A + B * C`
3. Expected Equation Breakdown: `(1010)₂ + (20)₈ × (2)₁₀` 
4. Expected Logic: Multiplication executes before addition. `10 + (16 × 2) = 42`.
5. Final Arithmetic Result: Binary: `101010`, Octal: `52`, Decimal: `42`, Hexadecimal: `2A`.

**Test Case 6: Parenthetical Grouping (Binary + Decimal + Hexadecimal)**
![Sample-6](sample-outputs/Sample-Output-6.png)
![Sample-6-1](sample-outputs/Sample-Output-6-1.png)
1. Inputs: `Input A = 10000` (Base 2), `Input B = 4` (Base 10), `Input C = 2` (Base 16)
2. Arithmetic Expression: `(A - B) / C`
3. Expected Equation Breakdown: `( (10000)₂ - (4)₁₀ ) ÷ (2)₁₆` 
4. Expected Logic: Parentheses force subtraction before division. `(16 - 4) ÷ 2 = 6`.
5. Final Arithmetic Result: Binary: `110`, Octal: `6`, Decimal: `6`, Hexadecimal: `6`.

**Test Case 7: Mixed Operators (Octal + Decimal + Hexadecimal)**
![Sample-7](sample-outputs/Sample-Output-7.png)
![Sample-7-1](sample-outputs/Sample-Output-7-1.png)
1. Inputs: `Input A = 12` (Base 8), `Input B = 5` (Base 10), `Input C = F` (Base 16)
2. Arithmetic Expression: `A * B - C`
3. Expected Equation Breakdown: `(12)₈ × (5)₁₀ - (F)₁₆` 
4. Expected Logic: Multiplication executes before subtraction. `(10 × 5) - 15 = 35`.
5. Final Arithmetic Result: Binary: `100011`, Octal: `43`, Decimal: `35`, Hexadecimal: `23`.

**Test Case 8: Complex Multi-Variable Expression (Binary + Octal + Hexadecimal + Binary)**
![Sample-8](sample-outputs/Sample-Output-8.png)
![Sample-8-1](sample-outputs/Sample-Output-8-1.png)
1. Inputs: `Input A = 1100` (Base 2), `Input B = 10` (Base 8), `Input C = A` (Base 16), `Input D = 11` (Base 2)
2. Arithmetic Expression: `(A + B - C) * D`
3. Expected Equation Breakdown: `( (1100)₂ + (10)₈ - (A)₁₆ ) × (11)₂` 
4. Expected Logic: Left-to-right inside parentheses, followed by multiplication. `(12 + 8 - 10) × 3 = 30`.
5. Final Arithmetic Result: Binary: `11110`, Octal: `36`, Decimal: `30`, Hexadecimal: `1E`.

**Test Case 9: Arithmetic Error Handling (Division by Zero)**
![Sample-9](sample-outputs/Sample-Output-9.png)
1. Inputs: `Input A = 25` (Base 10), `Input B = 0` (Base 2)
2. Arithmetic Expression: `A / B`
3. Expected Equation Breakdown: None.
4. Expected Output: Red error message stating "Error: Division by zero error." prevents the calculation.

**Test Case 10: Comprehensive Order of Operations (Mixed Bases)**
![Sample-10](sample-outputs/Sample-Output-10.png)
![Sample-10-1](sample-outputs/Sample-Output-10-1.png)
1. Inputs: `Input A = 14` (Hexadecimal), `Input B = 101` (Binary), `Input C = 4` (Octal), `Input D = 8` (Hexadecimal), `Input E = 10` (Binary)
2. Arithmetic Expression: `A + B * C - D / E`
3. Expected Equation Breakdown: `(14)₁₆ + (101)₂ × (4)₈ - (8)₁₆ ÷ (10)₂`
4. Expected Logic: Base conversions equate to (20 + 5 × 4 - 8 ÷ 2). Multiplication and division execute first `(5 × 4 = 20)` and `(8 ÷ 2 = 4)`, followed by addition and subtraction `(20 + 20 - 4) = 36`.
5. Final Arithmetic Result: Binary: `100100`, Octal: `44`, Decimal: `36`, Hexadecimal: `24`.

**Test Case 11: Fractional Input Calculation**
![Sample-11](sample-outputs/Sample-Output-11.png)
![Sample-11-1](sample-outputs/Sample-Output-11-1.png)
1. Inputs: `Input A = 23.5` (Decimal), `Input B = 10` (Decimal)
2. Arithmetic Expression: `A * B`
3. Expected Equation Breakdown: `(23.5)₁₀ × (10)₁₀`
4. Expected Logic: Multiplication of a fractional decimal by a whole decimal. `23.5 × 10 = 235`.
5. Final Arithmetic Result: Binary: `11101011`, Octal: `353`, Decimal: `235`, Hexadecimal: `EB`.

**Test Case 12: Non-Decimal Fractional Input Calculation**
![Sample-12](sample-outputs/Sample-Output-12.png)
![Sample-12-1](sample-outputs/Sample-Output-12-1.png)
1. Inputs: `Input A = 101.1` (Binary), `Input B = 1A` (Hexadecimal)
2. Arithmetic Expression: `A + B`
3. Expected Equation Breakdown: `(101.1)₂ + (1A)₁₆`
4. Expected Logic: Base conversions equate to (5.5 + 26). The fractional binary successfully adds to the whole hexadecimal. `5.5 + 26 = 31.5`.
5. Final Arithmetic Result: Binary: `11111.1`, Octal: `37.4`, Decimal: `31.5`, Hexadecimal: `1F.8`.

**Test Case 13: Standard Complement Generation (Base 10)**
![Sample-13](sample-outputs/Sample-Output-13.png)
1. Inputs: `Input A = 25` (Decimal)
2. Expected Logic: The system computes standard base conversions and dynamically generates the 8-bit padded 1's and 2's complements for the positive integer.
3. Expected Output: Binary: `11001`, Octal: `31`, Decimal: `25`, Hexadecimal: `19`, 1's Comp (Int): `11100110`, 2's Comp (Int): `11100111`.

**Test Case 14: Complement Subtraction (Positive Result / Minuend > Subtrahend)**
![Sample-14](sample-outputs/Sample-Output-14.png)
![Sample-13](sample-outputs/Sample-Output-14-1.png)
1. Inputs: Set Minuend dropdown to Variable `A` (Decimal `15`), Set Subtrahend to Variable `B` (Decimal `5`).
2. Expected Logic: The tool processes `15 - 5` using 8-bit representation.
3. 1's Complement Output: Converts S(5) to `00000101`, 1's comp `11111010`. Adds to M(15) `00001111`. Yields carry=1. Adds end-around carry to yield `00001010` (10).
4. 2's Complement Output: Converts S(5) to 2's comp `11111011`. Adds to M(15). Yields carry=1 (discarded), resulting directly in `00001010` (10).

**Test Case 15: Complement Subtraction (Negative Result / Minuend < Subtrahend)**
![Sample-15](sample-outputs/Sample-Output-15.png)
1. Inputs: Set Minuend dropdown to Variable `A` (Decimal `5`), Set Subtrahend to Variable `B` (Decimal `15`).
2. Expected Logic: The tool processes `5 - 15` using 8-bit representation.
3. 1's Complement Output: Converts S(15) to `00001111`, 1's comp `11110000`. Adds to M(5) `00000101`. Yields no carry. Final result is a negative number in 1's complement form: `11110101`.
4. 2's Complement Output: Converts S(15) to 2's comp `11110001`. Adds to M(5). Yields no carry. Final result is a negative number in 2's complement form: `11110110`.

**Test Case 16: Final Arithmetic Result with Complements**
![Sample-16](sample-outputs/Sample-Output-16.png)
![Sample-16-1](sample-outputs/Sample-Output-16-1.png)
1. Inputs: `Input A = 10` (Decimal), `Input B = 2` (Decimal)
2. Arithmetic Expression: `A - B`
3. Expected Equation Breakdown: `(10)₁₀ - (2)₁₀`
4. Expected Logic: The system computes `10 - 2 = 8`, then dynamically generates the complements for the final computed result.
5. Final Arithmetic Result: Binary: `1000`, Octal: `10`, Decimal: `8`, Hexadecimal: `8`, 1's Comp (Int): `11110111`, 2's Comp (Int): `11111000`.

**Code Screenshots**

![Code-SS-1](code-snippets/ss-1.png)
![Code-SS-2](code-snippets/ss-2.png)
![Code-SS-3](code-snippets/ss-3.png)
![Code-SS-4](code-snippets/ss-4.png)
![Code-SS-5](code-snippets/ss-5.png)
![Code-SS-6](code-snippets/ss-6.png)
![Code-SS-7](code-snippets/ss-7.png)
![Code-SS-8](code-snippets/ss-8.png)
![Code-SS-9](code-snippets/ss-9.png)
![Code-SS-10](code-snippets/ss-10.png)
![Code-SS-11](code-snippets/ss-11.png)
![Code-SS-12](code-snippets/ss-12.png)
![Code-SS-13](code-snippets/ss-13.png)
![Code-SS-14](code-snippets/ss-14.png)
![Code-SS-15](code-snippets/ss-15.png)
![Code-SS-16](code-snippets/ss-16.png)
![Code-SS-17](code-snippets/ss-17.png)
![Code-SS-18](code-snippets/ss-18.png)