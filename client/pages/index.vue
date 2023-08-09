<template>
	<v-container>
		<v-data-table :headers="headers" :items="launches" item-value="name" class="elevation-1">
			<template #top>
				<v-toolbar flat>
					<v-toolbar-title>SpaceX Launches</v-toolbar-title>
				</v-toolbar>
			</template>
			<template #[`item.launch_site`]="{ item }">
				<span v-if="item.columns.launch_site === null">No launch site available</span>
				<span v-else>{{ item.columns.launch_site.site_name }}</span>
			</template>
			<template #[`item.details`]="{ item }">
				<span v-if="item.columns.details === null">No details available</span>
				<span v-else>{{ item.columns.details }}</span>
			</template>
			<template #[`item.launch_date_utc`]="{ item }">
				<span>{{ new Date(item.columns.launch_date_utc).toLocaleString() }}</span>
			</template>
		</v-data-table>
	</v-container>
</template>

<script lang="ts" setup>
const headers = [
	{
		title: 'Mission Names',
		align: 'start',
		sortable: false,
		key: 'mission_name',
	},
	{ title: 'Launch date', sortable: false, key: 'launch_date_utc' },
	{ title: 'Launch site name', sortable: false, key: 'launch_site' },
	{ title: 'Rocket name', sortable: false, key: 'rocket.rocket_name' },
	{ title: 'Details', sortable: false, key: 'details' },
]
const query = gql`
	query getShips {
		launches {
			id
			mission_name
			launch_date_utc
			launch_site {
				site_name
			}
			rocket {
				rocket_name
			}
			details
		}
	}
`
const { data } = useAsyncQuery<{
	launches: {
		id: String
		mission_name: String
		launch_date_utc: Date
		site_name: String
		rocket_name: String
		details: String
	}
}>(query)

const launches = computed(() => data.value?.launches ?? [])
</script>

<style>
td {
	min-width: 220px;
}
</style>
