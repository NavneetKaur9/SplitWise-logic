```js
function processData(input) {
    let inputArr = input.split("\n");
    let personsArr = inputArr[0].split("");
    let g = new PersonGraph(personsArr);
    //add person
    for (let i = 0; i < personsArr.length; i++) {
        g.addPerson(personsArr[i]);
    }

    // note balance of each person 
    for (let i = 1; i < inputArr.length; i++) {
        let sdata = inputArr[i].split(" ");
        let person = sdata[0];
        let amtDebit = parseInt(sdata[1]);
        g.addDebitDetail(person, amtDebit);
    }

    g.calculateBalance();
    g.getCashFlow();
}

class PersonGraph {

    constructor(personsList) {
        this.personCount = personsList.length;
        this.adjList = new Map();
    }

    addPerson(person) {
        this.adjList.set(person, { debit: 0 });
    }

    addDebitDetail(person, dAmt) {
        this.adjList.get(person).debit = dAmt;
    }

    updateAmtDetail(person, Amt) {
        this.adjList.get(person).amt = Amt;
    }

    calculateBalance() {
        let keys = this.getKeys();
        let perHeadExppense = this.totalExpense() / this.personCount;

        for (let i of keys) {
            this.adjList.get(i).amt = this.adjList.get(i).debit - perHeadExppense;
        }
    }

    totalExpense() {
        let keys = this.getKeys();
        let totalExpenses = 0;

        for (let i of keys) {
            totalExpenses += this.adjList.get(i).debit
        }

        return totalExpenses;
    }

    getFirstKey() {
        return this.getKeys().next().value
    }

    max() {
        let maxAmtKey = this.getFirstKey();

        for (let i of this.getKeys()) {
            if (this.adjList.get(i).amt > this.adjList.get(maxAmtKey).amt) {
                maxAmtKey = i;
            }
        }
        return maxAmtKey;
    }

    getKeys() {
        return this.adjList.keys()
    }

    min() {
        let minAmtKey = this.getFirstKey();

        for (let i of this.getKeys()) {
            if (this.adjList.get(i).amt < this.adjList.get(minAmtKey).amt) {
                minAmtKey = i;
            }
        }
        return minAmtKey;
    }

    getCashFlow() {
        let maxCreditor = this.max();
        let maxDebitor = this.min();
        let creditorAmt = this.adjList.get(maxCreditor).amt;
        let debitorAmt = this.adjList.get(maxDebitor).amt;
        let min = Math.min(creditorAmt, -debitorAmt);

        if (creditorAmt == 0 && debitorAmt == 0) {
            return;
        }

        let updateCreditorAmt = creditorAmt - min;
        let updateDebitorAmt = debitorAmt + min;
        //Final Output: 
        console.log(maxDebitor, '->', maxCreditor, min);

        this.updateAmtDetail(maxCreditor, updateCreditorAmt);
        this.updateAmtDetail(maxDebitor, updateDebitorAmt);
        this.getCashFlow();
    }
}

let input = `ABCD
A 300
B 200
D 100`;

function main(input) {
    console.clear();
    processData(input);
}

main(input);
```
