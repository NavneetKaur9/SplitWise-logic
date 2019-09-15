```js
function processData(input) {
    let inputArr = input.split("\n");
    let personsArr = inputArr[0].split("");
    let g = personGraph(personsArr);
    //add person
    for(let i =0; i < personsArr.length; i++){
        g.addPerson(personsArr[i]);
    }
    
    // note balance of each person
    for( let i =1; i<inputArr.length; i++){
        let sdata= inputArr[i].split(" ");
        let person = sdata[0];
        let amtDebit = parseInt(sdata[1]);
        g.addDebitDetail(person,amtDebit);
    }   

    // let tE = g.totalExpense();
    // let perHead  = tE/personsArr.length;
    g.calculateBalance();

    // console.log(g);
    //
    g.getCashFlow();
} 

function personGraph(personsList){

    function addPerson(person){
        this.adjList.set(person,{debit:0});
    }

    function addDebitDetail(person,dAmt){
        this.adjList.get(person).debit = dAmt; 
    }

    function calculateBalance(){
        let keys = this.adjList.keys();
        let perHeadExppense = this.totalExpense()/this.personCount;
        
        for(let i of keys){
            this.adjList.get(i).amt= this.adjList.get(i).debit - perHeadExppense;
        }
    }

    function totalExpense(){
        let keys = this.adjList.keys();
        let totalExpenses = 0;

        for(let i of keys){
            totalExpenses+= this.adjList.get(i).debit
        }

        return totalExpenses;
    }
    
    function max(){
        // let max=this.adjList.values();
        // console.log(max);
        // for(let i of this.adjList.keys()){
        //     if(this.adjList.get(i).amt > )
        // }
    }

    function getCashFlow(){
        let max=this.adjList.get("A");
        console.log(max);
        //get maxCredit
        // let maxCredit = max(); 

        //get maxDebit
        // let maxDebit = 
    }

    return {
        personCount : personsList.length,
        adjList: new Map(),
        addPerson: addPerson,
        addDebitDetail : addDebitDetail, 
        totalExpense: totalExpense,
        calculateBalance:calculateBalance,
        getCashFlow: getCashFlow,
    }
}

process.stdin.resume();
process.stdin.setEncoding("ascii");
_input = "";
process.stdin.on("data", function (input) {
    _input += input;
});

process.stdin.on("end", function () {
   processData(_input);
});
```
