// Function to convert a number from a given base to decimal
function decodeValue(base, value) {
    return parseInt(value, parseInt(base));
}

// Lagrange Interpolation to find the polynomial's constant term
function lagrangeInterpolation(points) {
    const n = points.length;
    let c = 0; // This will hold the constant term

    for (let i = 0; i < n; i++) {
        let xi = points[i][0];
        let yi = points[i][1];

        // Calculate the Lagrange basis polynomial for each point
        let li = 1;
        for (let j = 0; j < n; j++) {
            if (j !== i) {
                li *= (0 - points[j][0]) / (xi - points[j][0]);
            }
        }

        c += yi * li; // Update the constant term
    }

    return Math.round(c); // Return the constant term, rounded to the nearest integer
}

// Main function to process the input and calculate the constant term
function findSecret(jsonInput) {
    const keys = jsonInput.keys;
    const n = keys.n;
    const k = keys.k;

    let points = [];

    // Decode the Y values and prepare the points array
    for (let i = 1; i <= n; i++) {
        const base = jsonInput[i].base;
        const value = jsonInput[i].value;
        const x = i; // Using the key as x
        const y = decodeValue(base, value); // Decoding y value

        points.push([x, y]);
    }

    // We only need the first k points for interpolation
    const secret = lagrangeInterpolation(points.slice(0, k));
    console.log(`The secret (constant term c) is: ${secret}`);
}

// Example JSON input
const jsonInput1 = {
    "keys": {
        "n": 4,
        "k": 3
    },
    "1": {
        "base": "10",
        "value": "4"
    },
    "2": {
        "base": "2",
        "value": "111"
    },
    "3": {
        "base": "10",
        "value": "12"
    },
    "4": {
        "base": "4",
        "value": "213"
    }
};

const jsonInput2 = {
    "keys": {
        "n": 10,
        "k": 7
    },
    "1": {
        "base": "6",
        "value": "13444211440455345511"
    },
    "2": {
        "base": "15",
        "value": "aed7015a346d63"
    },
    "3": {
        "base": "15",
        "value": "6aeeb69631c227c"
    },
    "4": {
        "base": "16",
        "value": "e1b5e05623d881f"
    },
    "5": {
        "base": "8",
        "value": "316034514573652620673"
    },
    "6": {
        "base": "3",
        "value": "2122212201122002221120200210011020220200"
    },
    "7": {
        "base": "3",
        "value": "20120221122211000100210021102001201112121"
    },
    "8": {
        "base": "6",
        "value": "20220554335330240002224253"
    },
    "9": {
        "base": "12",
        "value": "45153788322a1255483"
    },
    "10": {
        "base": "7",
        "value": "1101613130313526312514143"
    }
};

// Run the function for both test cases
findSecret(jsonInput1);
findSecret(jsonInput2);
h
