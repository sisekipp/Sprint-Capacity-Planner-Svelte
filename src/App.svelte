<script>
    import {beforeUpdate} from 'svelte';
    import {format, addWeeks, parse, differenceInWeeks} from 'date-fns';

    import './style.scss';

    let data = {
        startDate: format(new Date(), 'yyyy-MM-dd'),
        endDate: format(addWeeks(new Date(), 1), 'yyyy-MM-dd'),
        workingHours: 8,
        workWeek: 5,
        capacityForTask: 60,
        sprintLength: 1,
        teamMember: [
            {
                name: 'Sebastian',
                workingTimeInPercent: 100,
                daysOf: 0

            }
        ],
    };
    beforeUpdate(() => {
        calcSprintLength();
        console.log(data);
    });

    let calcSprintLength = () => {
        data.sprintLength = differenceInWeeks(parse(data.endDate, 'yyyy-MM-dd', new Date()), parse(data.startDate, 'yyyy-MM-dd', new Date()));
    };


</script>

<nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
        <span class="navbar-item">Sprint Capacity Planner</span>
    </div>
</nav>

<main>
    <section class="section">
        <div class="container is-fluid">
            <form>
                <div class="field">
                    <label class="label">Start</label>
                    <div class="control">
                        <input class="input" type="date" bind:value={data.startDate}>
                    </div>
                </div>
                <div class="field">
                    <label class="label">End</label>
                    <div class="control">
                        <input class="input" type="date" bind:value={data.endDate}>
                    </div>
                </div>
                <div class="field">
                    <label class="label">Working hours</label>
                    <div class="control">
                        <input class="input" type="number" bind:value={data.workingHours}>
                    </div>
                </div>
                <div class="field">
                    <label class="label">Workweek</label>
                    <div class="control">
                        <input class="input" type="number" bind:value={data.workWeek}>
                    </div>
                </div>
                <div class="field">
                    <label class="label">Capacity for tasks</label>
                    <div class="control">
                        <input class="input" type="number" bind:value={data.capacityForTask}>
                    </div>
                </div>
            </form>
        </div>
    </section>

    <section class="section">
        <div class="container is-fluid">
            <table class="table is-fullwidth">
                <thead>
                <tr>
                    <th>Name</th>
                    <th>Days of</th>
                    <th>Working time</th>
                </tr>
                </thead>
                <tbody>
                {#each data.teamMember as member}
                    <tr>
                        <td>{member.name}</td>
                        <td>{member.daysOf}</td>
                        <td>{member.workingTimeInPercent}%</td>
                    </tr>
                {/each}
                </tbody>
            </table>
        </div>
    </section>


</main>

<style>

</style>
