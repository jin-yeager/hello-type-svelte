<script lang="ts">
    let l1: string = "__"
    let l2: string = "__"
    let l3: string = "__"
    let l4: string = "__"
    let l5: string = "__"
    let l6: string = "__"

    const myclick = () => {
        let arr45 :number[] = [
            1,2,3,4,5,	6,7,8,9,10,
            11,12,13,14,15,	16,17,18,19,20,
            21,22,23,24,25,	26,27,28,29,30,
            31,32,33,34,35,	36,37,38,39,40,
            41,42,43,44,45
        ];

        for (let i:number = 0; i < 1000; i++) {
            let rnd : number = parseInt((Math.random() * 45).toString());
            let temp : number = arr45[0];
            arr45[0] = arr45[rnd];
            arr45[rnd] = temp;
        }

        for (let i:number = 0; i < 6; i++) {
            for (let j = 0; j < 6; j++) {
                if (arr45[i] < arr45[j]) {
                    let a:number = arr45[i];
                    let b:number = arr45[j]
                    arr45[i] = b;
                    arr45[j] = a;
                }
            }
        }


        l1 = arr45[0].toString()
        l2 = arr45[1].toString()
        l3 = arr45[2].toString()
        l4 = arr45[3].toString()
        l5 = arr45[4].toString()
        l6 = arr45[5].toString()
    }

</script>
<table>
    <tbody>
    <tr>
        <td><span>{l1}</span></td>
        <td><span>{l2}</span></td>
        <td><span>{l3}</span></td>
        <td><span>{l4}</span></td>
        <td><span>{l5}</span></td>
        <td><span>{l6}</span></td>
    </tr>
    <tr>
        <td colspan="6">
            <button onclick={myclick}>로또생성하기</button>
        </td>
    </tr>

    </tbody>

</table>

<style>
    table,tr,td{
        border: 1px solid grey ;
    }
    td{
        width: 20px;
    }
</style>