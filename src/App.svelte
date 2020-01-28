<script>
    import {beforeUpdate} from 'svelte';
    import {format, addWeeks, parse, differenceInWeeks} from 'date-fns';

    import './style.scss';

    import  Icon  from 'fa-svelte'
    import { faEllipsisV, faPlus } from '@fortawesome/free-solid-svg-icons'

    let dotIcon = faEllipsisV;
    let plusIcon = faPlus;

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

<nav class="navbar navbar-dark" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
        <span class="navbar-item">Sprint Capacity Planner</span>
    </div>
</nav>

<main>
    <div class="container-fluid">
        <section>
            <div class="row">
                <div class="col-sm-2">

                    <form>
                        <div class="form-group">
                            <label for="startDate">Start</label>
                            <input id="startDate" class="form-control" type="date" bind:value={data.startDate}>
                        </div>
                        <div class="form-group">
                            <label for="">End</label>
                            <input id="endDate" class="form-control" type="date" bind:value={data.endDate}>
                        </div>
                        <div class="form-group">
                            <label for="workingHours">Working hours</label>
                            <input id="workingHours" class="form-control" type="number" bind:value={data.workingHours}>
                        </div>
                        <div class="form-group">
                            <label for="workWeek">Workweek</label>
                            <input id="workWeek" class="form-control" type="number" bind:value={data.workWeek}>
                        </div>
                        <div class="form-group">
                            <label for="capacityForTasks">Capacity for tasks</label>
                            <input id="capacityForTasks" class="form-control" type="number" bind:value={data.capacityForTask}>
                            <small class="form-text text-muted">In percent</small>
                        </div>
                    </form>

                </div>
            </div>
        </section>

        <section>
            <div class="table-buttons">
                <button type="button" class="btn btn-secondary btn-sm align-middle"><Icon icon="{plusIcon}"/>Add Team Member</button>
            </div>

            <div class="table-responsive">
                <table class="table table-bordered">
                    <thead class="thead-dark">
                    <tr>
                        <th></th>
                        <th>Name</th>
                        <th>Days of</th>
                        <th>Working time</th>
                    </tr>
                    </thead>
                    <tbody>
                    {#each data.teamMember as member}
                        <tr class="table-light">
                            <td>
                                <div class="align-middle btn-group dropright">
                                    <button type="button" class="btn btn-sm" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                                        <Icon icon="{dotIcon}"/>
                                    </button>
                                    <div class="dropdown-menu">
                                        <a class="dropdown-item" href="#">Edit</a>
                                        <a class="dropdown-item" href="#">Delete</a>
                                    </div>
                                </div>
                            </td>
                            <td class="align-middle">{member.name}</td>
                            <td class="align-middle">{member.daysOf}</td>
                            <td class="align-middle">{member.workingTimeInPercent}%</td>
                        </tr>
                    {/each}
                    </tbody>
                </table>
                <p class="text-right">Sprint duration: {data.sprintLength} weeks / Capacity: 0 hours</p>
            </div>
        </section>
    </div>


</main>
