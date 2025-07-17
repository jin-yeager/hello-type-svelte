<script lang="ts">
import axios, { type AxiosResponse } from "axios";
import { onMount } from "svelte";
import { navigate } from "svelte-routing";

interface Employee {
    e_id: string;
    e_name: string;
    gen: string;
    addr: string;
}

interface DetailResponse {
    vo: Employee;
}

interface DeleteResponse {
    cnt: number;
}

let vo: Employee = { e_id: "", e_name: "", gen: "", addr: "" };

onMount(async (): Promise<void> => {
    try {
        const urlParams = new URLSearchParams(window.location.search);
        const pe_id: string | null = urlParams.get("e_id");

        if (!pe_id) {
            alert("잘못된 접근입니다.");
            navigate("/emp_list.do");
            return;
        }

        const resp: AxiosResponse<DetailResponse> = await axios.get("http://localhost:80/emp_detail.ajax", {
            params: {
                e_id: pe_id,
            },
        });

        console.log(resp);
        vo = resp.data.vo;
    } catch (error) {
        console.error('직원 정보 조회 중 오류:', error);
        alert("직원 정보를 불러오는데 실패했습니다.");
    }
});

const fn_mod = async (): Promise<void> => {
    navigate(`/emp_mod.do?e_id=${vo.e_id}`);
};

const fn_del = async (): Promise<void> => {
    const flag_c: boolean = confirm(
        "한번 지워진 데이터는 복구불가합니다.\n삭제하시겠습니까?"
    );

    if (!flag_c) {
        return;
    }

    try {
        const resp: AxiosResponse<DeleteResponse> = await axios.post("http://localhost:80/emp_del.ajax", vo);
        const cnt: number = resp.data.cnt;

        if (cnt === 1) {
            alert("정상적으로 삭제되었습니다.");
            navigate(`/emp_list.do`);
        } else {
            alert("삭제도중 문제가 생겼습니다.");
        }
    } catch (error) {
        console.error('직원 삭제 중 오류:', error);
        alert("삭제도중 문제가 생겼습니다.");
    }
};
</script>

EMP DETAIL 화면
<table>
<tbody>
    <tr>
        <td>사번</td>
<td>
{vo.e_id}
</td>
</tr>
<tr>
<td>이름</td>
<td>
{vo.e_name}
</td>
</tr>
<tr>
<td>성별</td>
<td>
{vo.gen}
</td>
</tr>
<tr>
<td>주소</td>
<td>
{vo.addr}
</td>
</tr>
<tr>
<td colspan="2">
<input type="button" value="수정" on:click={fn_mod} />
<input type="button" value="삭제" on:click={fn_del} />
</td>
</tr>
</tbody>
</table>

<style>
table,
    tr,
    td {
    border: 1px solid darkolivegreen;
    text-align: center;
}

input[type="text"] {
    width: 200px;
}

a {
    color: blue;
    text-decoration: underline;
}
</style>