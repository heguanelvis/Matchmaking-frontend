<template>
    <div class="admin-calendar-home shadow rounded">
        <div class="d-flex">
            <ul class="list-unstyled">
                <li class="text-center header-grid">
                    <p>Schedule</p>
                    <p>Company</p>
                </li>
                <li
                    class="text-center d-flex"
                    v-for="tindex in 10"
                    :key="tindex"
                >
                    <p>
                        {{ calculateTime(tindex - 1) }} -
                        {{ calculateTime(tindex) }}
                    </p>
                </li>
            </ul>
            <ul
                v-for="schedule in allSchedules"
                :key="schedule.employerId"
                class="list-unstyled"
            >
                <li class="text-center d-flex">
                    <p>{{ schedule.employerName }}</p>
                </li>
                <li class="text-center d-flex" v-for="index in 10" :key="index">
                    <p>
                        {{
                            index === 5
                                ? "Lunch"
                                : schedule.studentInterviews[index]
                                ? schedule.studentInterviews[index]
                                      .studentFirstName +
                                  " " +
                                  schedule.studentInterviews[index]
                                      .studentLastName
                                : ""
                        }}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import auth from "../auth";
import moment from "moment";

export default {
    data() {
        return {
            allSchedules: {}
        };
    },
    methods: {
        loadAllSchedulesJsonData() {
            fetch(`${process.env.VUE_APP_REMOTE_API}/api/allschedules`, {
                method: "GET",
                headers: {
                    Authorization: "Bearer " + auth.getToken(),
                    Accept: "application/json",
                    "Content-Type": "application/json"
                }
            })
                .then(response => {
                    return response.json();
                })
                .then(allSchedulesData => {
                    this.allSchedules = allSchedulesData;
                });
        },
        calculateTime(appointmentDateTime) {
            let dayStart = moment("1115", "HHmm");
            dayStart.add(30 * appointmentDateTime, "minutes");
            return dayStart.format("HH:mm");
        }
    },
    created() {
        this.loadAllSchedulesJsonData();
    }
};
</script>

<style scoped>
.admin-calendar-home {
    background: rgba(248, 249, 250, 0.94);
}

.admin-calendar-home > div {
    overflow-x: scroll;
}

ul {
    padding: 0;
}

ul:first-of-type {
    font-weight: bold;
}

.header-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    grid-template-areas:
        ". company"
        "schedule .";
    background-image: linear-gradient(
        to top right,
        var(--teblue) calc(50% - 1px),
        var(--tegray),
        var(--teblue) calc(50% + 1px)
    ) !important;
}

.header-grid > p:first-of-type {
    grid-area: schedule;
    place-self: center;
}

.header-grid > p:last-of-type {
    grid-area: company;
    place-self: center;
}

ul li {
    float: bottom;
    padding: 10 20px;
    border: 1px solid #000;
    height: 3rem;
    width: 10rem;
}

ul li:nth-of-type(1) {
    background: var(--teblue);
    font-weight: bold;
    opacity: 0.9;
}

ul li:nth-of-type(6) {
    background: var(--tegreen);
    opacity: 0.9;
}

ul li > p {
    margin: auto;
}

ul li:not(:first-of-type) {
    border-top: none;
}

ul:not(:last-of-type) li {
    border-right: none;
}

@media only screen and (max-width: 600px) {
    .admin-calendar-home {
        margin-top: 7rem;
        margin-left: 1rem;
        margin-right: 1rem;
        margin-bottom: 2rem;
        padding: 1rem;
    }
}

@media only screen and (min-width: 600px) {
    .admin-calendar-home {
        margin-top: 8rem;
        margin-left: 2rem;
        margin-right: 2rem;
        margin-bottom: 2rem;
        padding: 2rem;
    }
}

@media only screen and (min-width: 992px) {
    .admin-calendar-home {
        padding: 5rem;
        margin: 10rem;
    }
}
</style>
