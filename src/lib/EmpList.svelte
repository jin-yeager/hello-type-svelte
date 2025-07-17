<script lang="ts">
import axios, { type AxiosResponse } from 'axios';
import { onMount } from 'svelte';
import { navigate } from 'svelte-routing';

interface Employee {
    e_id: string;
    e_name: string;
    gen: string;
    addr: string;
}

interface ListResponse {
    list: Employee[];
}

let list: Employee[] = [
    { e_id: '1', e_name: '1', gen: '1', addr: '1' },
    { e_id: '2', e_name: '2', gen: '2', addr: '2' },
    { e_id: '3', e_name: '3', gen: '3', addr: '3' },
];

onMount(async (): Promise<void> => {
    await fn_list();
});

const fn_list = async (): Promise<void> => {
    try {
        const resp: AxiosResponse<ListResponse> =
            await axios.get('http://localhost:80/emp_list.ajax');
        list = resp.data.list;
    } catch (error) {
        console.error('직원 목록 조회 중 오류:', error);
        alert('직원 목록을 불러오는데 실패했습니다');
    }
};

const fn_one = async (e: MouseEvent): Promise<void> => {
    const target = e.currentTarget as HTMLElement;
    const e_id = target.dataset.e_id;
    if (e_id) {
        navigate(`/emp_detail.do?e_id=${e_id}`);
    }
};

const fn_add = (): void => {
    navigate('/emp_add.do');
};
</script>

<main>
<h1>EMP LIST 화면</h1>
<table>
<thead>
    <tr>
        <td>사번</td>
<td>이름</td>
<td>전화</td>
<td>이메일</td>
</tr>
</thead>
<tbody>
{#each list as vo}
<tr>
<td>
    <a on:click={fn_one} data-e_id={vo.e_id}>{vo.e_id}</a>
    </td>
    <td>{vo.e_name}</td>
    <td>{vo.gen}</td>
    <td>{vo.addr}</td>
    </tr>
{/each}
</tbody>
</table>
<input type="button" value="추가" on:click={fn_add} />
</main>

<style>
table,
    tr,
    td {
    border: 1px solid darkolivegreen;
    text-align: center;
}

    input[type='text'] {
    width: 50px;
}

    a {
    color: blue;
    text-decoration: underline;
}
    </style>
