<script>
	import {Button} from "smelte";
	import { TextField } from "smelte";
	import { DataTable } from "smelte";
	import "./app.css";
	let totalCount, totalElectricity;
	let lastRoomOneDegree, lastRoomTwoDegree, lastRoomThreeDegree, lastRoomFourDegree;
	let todayRoomOneDegree, todayRoomTwoDegree, todayRoomThreeDegree, todayRoomFourDegree;
	let data = [];
	const dateStr = new Date().toDateString();
	// test data
	// totalElectricity = 496;
	// totalCount = 267.3;
	// lastRoomOneDegree = 432.1;
	// lastRoomTwoDegree = 533.2;
	// lastRoomThreeDegree = 1481.8;
	// lastRoomFourDegree = 510.7;

	// todayRoomOneDegree = 473.9;
	// todayRoomTwoDegree = 553.9;
	// todayRoomThreeDegree = 1682.4;
	// todayRoomFourDegree = 612.8;
	// test data
	function generate() {
		const oneUsed = todayRoomOneDegree - lastRoomOneDegree;
		const twoUsed = todayRoomTwoDegree - lastRoomTwoDegree;
		const threeUsed = todayRoomThreeDegree - lastRoomThreeDegree;
		const fourUsed = todayRoomFourDegree - lastRoomFourDegree;
		const publicUsedEveryone = (totalElectricity - oneUsed - twoUsed - threeUsed - fourUsed) / 4;
		const unitPrice = totalCount / totalElectricity;

		const publicCostEveryone = publicUsedEveryone * unitPrice;

		const oneCost = unitPrice * oneUsed + publicCostEveryone;
		const twoCost = unitPrice * twoUsed + publicCostEveryone;
		const threeCost = unitPrice * threeUsed + publicCostEveryone;
		const fourCost = unitPrice * fourUsed + publicCostEveryone;


		const resultMap = {
			1: {used: oneUsed, cost: oneCost},
			2: {used: twoUsed, cost: twoCost},
			3: {used: threeUsed, cost: threeCost},
			4: {used: fourUsed, cost: fourCost}
		}
		const res = [];
		for(let i = 1; i <= 4; i++) {
			res.push({
				id: i,
				price: parseFloat(unitPrice).toFixed(3),
				electricity: parseFloat(resultMap[i]["used"]).toFixed(2),
				public: parseFloat(publicUsedEveryone).toFixed(2) + '/' + parseFloat(publicCostEveryone).toFixed(2),
				cost: parseFloat(resultMap[i]["cost"]).toFixed(3)
			})
		}
		data = res;
	}
</script>

<main>
	<h1 class="header_title">Electricity Bill</h1>
	<span>{dateStr}</span>
	<div class="elec_form">
		<h2 class="elec_sub_title">Last Month Basic</h2>
		<TextField bind:value={totalElectricity} label="Total Electricity" hint="度" outlined></TextField>
		<TextField bind:value={totalCount} label="Total Cost" hint="元" outlined></TextField>
	</div>
	<div class="elec_form">
		<h2 class="elec_sub_title">Last Month</h2>
		<TextField bind:value={lastRoomOneDegree} label="Room One" hint="度" outlined></TextField>
		<TextField bind:value={lastRoomTwoDegree} label="Room Two" hint="度" outlined></TextField>
		<TextField bind:value={lastRoomThreeDegree} label="Room Three" hint="度" outlined></TextField>
		<TextField bind:value={lastRoomFourDegree} label="Room Four" hint="度" outlined></TextField>
	</div>

	<div class="elec_form">
		<h2 class="elec_sub_title">This Month</h2>
		<TextField bind:value={todayRoomOneDegree} label="Room One" hint="度" outlined></TextField>
		<TextField bind:value={todayRoomTwoDegree} label="Room Two" hint="度" outlined></TextField>
		<TextField bind:value={todayRoomThreeDegree} label="Room Three" hint="度" outlined></TextField>
		<TextField bind:value={todayRoomFourDegree} label="Room Four" hint="度" outlined></TextField>
	</div>
	<Button on:click={generate}>Generate Bill</Button>
	<div class="table_box">
		<DataTable
			{data}
			perPage={10}
			pagination={false}
			columns={[
				{ label: "Room", field: "id", sortable: false, editable: false, class: "id_column"},
				{ label: "Cost(￥)", field: "cost", sortable: false, editable: false, class: "cost_column common_column"},
				{ label: "Price", field: "price", sortable: false, editable: false, class: "common_column"},
				{ label: "Electricity", field: "electricity", sortable: false, editable: false, class: "common_column"},
				{ label: "Pulic(kwh/￥)", field: "public", sortable: false, editable: false, class: "common_column"},
			]}
		/>
	</div>

</main>

<style>
	main {
		text-align: center;
		padding: .1rem;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 1.8rem;
		letter-spacing: .2rem;
	}

	.header_title {
		margin-top: 2rem;
	}
	.elec_form {
		padding: 1rem;
	}
	.elec_sub_title {
		font-size: 1.4rem;
	}
	.table_box {
    	margin: 2rem 0;
		display: flex;
		justify-content: center;
		overflow-x: auto;
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>