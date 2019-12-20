<template>
    <div class="employer-calendar-home shadow rounded">
        <h2 v-if="!hasSchedule" class="text-center">
            Sorry, the schedule has not been assigned yet.
        </h2>
        <h2 v-if="hasSchedule" class="text-center">
            {{ employerSchedule.employerName }}
        </h2>
        <ul v-if="hasSchedule" class="timeline">
            <li v-for="index in 10" :key="index">
                <div :class="index % 2 === 0 ? `direction-r` : `direction-l`">
                    <div class="flag-wrapper">
                        <span
                            :class="
                                employerSchedule.studentInterviews[index]
                                    ? `flag`
                                    : `flag grayout`
                            "
                        >
                            <router-link
                                v-if="employerSchedule.studentInterviews[index]"
                                :to="
                                    `/students/${employerSchedule.studentInterviews[index].studentId}`
                                "
                                style="color: var(--tegray)"
                                >{{
                                    employerSchedule.studentInterviews[index]
                                        ? employerSchedule.studentInterviews[
                                              index
                                          ].studentFirstName +
                                          " " +
                                          employerSchedule.studentInterviews[
                                              index
                                          ].studentLastName
                                        : ""
                                }}</router-link
                            ><fragment v-else>Break Time</fragment></span
                        >
                        <span class="time-wrapper"
                            ><span class="time"
                                >{{ calculateTime(index - 1) }} to
                                {{ calculateTime(index) }}</span
                            ></span
                        >
                    </div>
                    <div class="desc">
                        <a
                            :href="
                                `mailto:${
                                    employerSchedule.studentInterviews[index]
                                        ? employerSchedule.studentInterviews[
                                              index
                                          ].studentEmail
                                        : null
                                }`
                            "
                            >{{
                                employerSchedule.studentInterviews[index]
                                    ? employerSchedule.studentInterviews[index]
                                          .studentEmail
                                    : ""
                            }}</a
                        >
                    </div>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
import auth from "../auth";
import moment from "moment";

export default {
    data() {
        return {
            hasSchedule: false,
            employerSchedule: {
                employerId: "",
                employerName: "",
                tableCount: "",
                daysToAttend: "",
                studentInterviews: {
                    "1": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "2": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "3": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "4": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "5": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "6": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "7": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "8": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "9": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    },
                    "10": {
                        studentId: "",
                        studentFirstName: "",
                        studentLastName: "",
                        studentEmail: "",
                        studentLinkedinUrl: "",
                        studentImgUrl: ""
                    }
                }
            }
        };
    },
    methods: {
        loadEmployerScheduleJsonData() {
            fetch(`${process.env.VUE_APP_REMOTE_API}/api/employer/schedule`, {
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
                .then(employerScheduleData => {
                    this.employerSchedule = employerScheduleData;
                    if (employerScheduleData.employerName) {
                        this.hasSchedule = true;
                    }
                });
        },
        calculateTime(appointmentDateTime) {
            let dayStart = moment("1115", "HHmm");
            dayStart.add(30 * appointmentDateTime, "minutes");
            return dayStart.format("HH:mm");
        }
    },
    created() {
        this.loadEmployerScheduleJsonData();
    }
};
</script>

<style scoped>
.grayout {
    color: rgb(185, 185, 185);
}

.employer-calendar-home {
    background: rgba(248, 249, 250, 0.94);
    padding: 5rem;
    margin: 10rem;
}

/* ================ The Timeline ================ */
.timeline {
    position: relative;
    width: 660px;
    margin: 0 auto;
    margin-top: 20px;
    padding: 1em 0;
    list-style-type: none;
}

.timeline:before {
    position: absolute;
    left: 50%;
    top: 0;
    content: " ";
    display: block;
    width: 6px;
    height: 100%;
    margin-left: -3px;
    background: rgb(80, 80, 80);
    background: -moz-linear-gradient(
        top,
        rgba(80, 80, 80, 0) 0%,
        rgb(80, 80, 80) 8%,
        rgb(80, 80, 80) 92%,
        rgba(80, 80, 80, 0) 100%
    );
    background: -webkit-gradient(
        linear,
        left top,
        left bottom,
        color-stop(0%, rgba(30, 87, 153, 1)),
        color-stop(100%, rgba(125, 185, 232, 1))
    );
    background: -webkit-linear-gradient(
        top,
        rgba(80, 80, 80, 0) 0%,
        rgb(80, 80, 80) 8%,
        rgb(80, 80, 80) 92%,
        rgba(80, 80, 80, 0) 100%
    );
    background: -o-linear-gradient(
        top,
        rgba(80, 80, 80, 0) 0%,
        rgb(80, 80, 80) 8%,
        rgb(80, 80, 80) 92%,
        rgba(80, 80, 80, 0) 100%
    );
    background: -ms-linear-gradient(
        top,
        rgba(80, 80, 80, 0) 0%,
        rgb(80, 80, 80) 8%,
        rgb(80, 80, 80) 92%,
        rgba(80, 80, 80, 0) 100%
    );
    background: linear-gradient(
        to bottom,
        rgba(80, 80, 80, 0) 0%,
        rgb(80, 80, 80) 8%,
        rgb(80, 80, 80) 92%,
        rgba(80, 80, 80, 0) 100%
    );

    z-index: 5;
}

.timeline li {
    padding: 1em 0;
}

.timeline li:after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}

.direction-l {
    position: relative;
    width: 300px;
    float: left;
    text-align: right;
}

.direction-r {
    position: relative;
    width: 300px;
    float: right;
}

.flag-wrapper {
    position: relative;
    display: inline-block;

    text-align: center;
}

.flag {
    position: relative;
    display: inline;
    background: rgb(248, 248, 248);
    padding: 6px 10px;
    border-radius: 5px;

    font-weight: 600;
    text-align: left;
}

.direction-l .flag {
    -webkit-box-shadow: -1px 1px 1px rgba(0, 0, 0, 0.15),
        0 0 1px rgba(0, 0, 0, 0.15);
    -moz-box-shadow: -1px 1px 1px rgba(0, 0, 0, 0.15),
        0 0 1px rgba(0, 0, 0, 0.15);
    box-shadow: -1px 1px 1px rgba(0, 0, 0, 0.15), 0 0 1px rgba(0, 0, 0, 0.15);
}

.direction-r .flag {
    -webkit-box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.15),
        0 0 1px rgba(0, 0, 0, 0.15);
    -moz-box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.15),
        0 0 1px rgba(0, 0, 0, 0.15);
    box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.15), 0 0 1px rgba(0, 0, 0, 0.15);
}

.direction-l .flag:before,
.direction-r .flag:before {
    position: absolute;
    top: 50%;
    right: -40px;
    content: " ";
    display: block;
    width: 12px;
    height: 12px;
    margin-top: -10px;
    background: #fff;
    border-radius: 10px;
    border: 4px solid var(--teblue);
    z-index: 10;
}

.direction-r .flag:before {
    left: -40px;
}

.direction-l .flag:after {
    content: "";
    position: absolute;
    left: 100%;
    top: 50%;
    height: 0;
    width: 0;
    margin-top: -8px;
    border: solid transparent;
    border-left-color: rgb(248, 248, 248);
    border-width: 8px;
    pointer-events: none;
}

.direction-r .flag:after {
    content: "";
    position: absolute;
    right: 100%;
    top: 50%;
    height: 0;
    width: 0;
    margin-top: -8px;
    border: solid transparent;
    border-right-color: rgb(248, 248, 248);
    border-width: 8px;
    pointer-events: none;
}

.time-wrapper {
    display: inline;

    line-height: 1em;
    font-size: 0.66666em;
    font-weight: bold;
    color: var(--teblue);
    vertical-align: middle;
}

.direction-l .time-wrapper {
    float: left;
}

.direction-r .time-wrapper {
    float: right;
}

.time {
    display: inline-block;
    padding: 4px 6px;
    background: rgb(248, 248, 248);
}

.desc {
    margin: 1em 0.75em 0 0;

    font-size: 0.77777em;
    font-style: italic;
    line-height: 1.5em;
}

.direction-r .desc {
    margin: 1em 0 0 0.75em;
}

/* ================ Timeline Media Queries ================ */
@media screen and (max-width: 660px) {
    .employer-calendar-home {
        padding: 1rem;
        margin: 7rem 1rem;
    }

    .timeline {
        width: 100%;
        padding: 4em 0 1em 0;
    }

    .timeline li {
        padding: 2em 0;
    }

    .direction-l,
    .direction-r {
        float: none;
        width: 100%;

        text-align: center;
    }

    .flag-wrapper {
        text-align: center;
    }

    .flag {
        background: rgb(255, 255, 255);
        z-index: 15;
    }

    .direction-l .flag:before,
    .direction-r .flag:before {
        position: absolute;
        top: -30px;
        left: 50%;
        content: " ";
        display: block;
        width: 12px;
        height: 12px;
        margin-left: -9px;
        background: #fff;
        border-radius: 10px;
        border: 4px solid var(--teblue);
        z-index: 10;
    }

    .direction-l .flag:after,
    .direction-r .flag:after {
        content: "";
        position: absolute;
        left: 50%;
        top: -8px;
        height: 0;
        width: 0;
        margin-left: -8px;
        border: solid transparent;
        border-bottom-color: rgb(255, 255, 255);
        border-width: 8px;
        pointer-events: none;
    }

    .time-wrapper {
        display: block;
        position: relative;
        margin: 4px 0 0 0;
        z-index: 14;
    }

    .direction-l .time-wrapper {
        float: none;
    }

    .direction-r .time-wrapper {
        float: none;
    }

    .desc {
        position: relative;
        margin: 1em 0 0 0;
        padding: 1em;
        background: rgb(245, 245, 245);
        -webkit-box-shadow: 0 0 1px rgba(0, 0, 0, 0.2);
        -moz-box-shadow: 0 0 1px rgba(0, 0, 0, 0.2);
        box-shadow: 0 0 1px rgba(0, 0, 0, 0.2);

        z-index: 15;
    }

    .direction-l .desc,
    .direction-r .desc {
        position: relative;
        margin: 1em 1em 0 1em;
        padding: 1em;

        z-index: 15;
    }
}

@media screen and (min-width: 660px) {
    .employer-calendar-home {
        padding: 0.5rem;
        margin: 8rem 0.8rem;
    }
}

@media screen and (min-width: 992px) {
    .employer-calendar-home {
        padding: 1rem;
        margin: 9rem 3rem;
    }
}

@media screen and (min-width: 1200px) {
    .employer-calendar-home {
        padding: 4rem;
        margin: 10rem 8rem;
    }
}
</style>
