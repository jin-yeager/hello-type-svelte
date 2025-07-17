<script lang="ts">
    import axios, { type AxiosResponse } from 'axios';
    import { navigate } from 'svelte-routing';

    interface Employee {
        e_id: string;
        e_name: string;
        gen: string;
        addr: string;
    }

    interface AddResponse {
        cnt: number;
    }

    let vo: Employee = {
        e_id: '',
        e_name: '',
        gen: '',
        addr: ''
    };

    const fn_add = async (): Promise<void> => {
        try {
            const resp: AxiosResponse<AddResponse> =
                await axios.post('http://localhost:80/emp_add.ajax', vo);
            if (resp.data.cnt === 1) {
                alert('정상적으로 추가되었습니다.');
                navigate('/emp_list.do');
            } else {
                alert('추가 도중 문제가 생겼습니다.');
            }
        } catch (error) {
            console.error('추가 중 오류:', error);
            alert('추가 중 오류가 발생했습니다.');
        }
    };
</script>

<main>
    <h1>사원 추가 화면</h1>
    <table>
        <tbody>
        <tr>
            <td>사번</td>
            <td>
                자동추가
            </td>
        </tr>
        <tr>
            <td>이름</td>
            <td><input type="text" bind:value={vo.e_name} /></td>
        </tr>
        <tr>
            <td>성별</td>
            <td><input type="text" bind:value={vo.gen} /></td>
        </tr>
        <tr>
            <td>주소</td>
            <td><input type="text" bind:value={vo.addr} /></td>
        </tr>
        </tbody>
    </table>
    <div class="actions">
        <button type="button" on:click={fn_add}>추가</button>
    </div>
</main>

<style>
    table {
        border-collapse: collapse;
        margin: 1em 0;
    }

    td {
        border: 1px solid darkolivegreen;
        padding: 0.5em;
        text-align: center;
    }

    input[type="text"] {
        width: 100%;
        box-sizing: border-box;
    }

    .actions {
        text-align: center;
        margin-top: 1em;
    }

    button {
        padding: 0.5em 1em;
        cursor: pointer;
        border: none;
        border-radius: 4px;
        background-color: #4a7c59;
        color: white;
    }
</style>
