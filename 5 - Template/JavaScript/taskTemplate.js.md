```
class taskTemplate {

    constructor (nameOfTask, stateOfTask, typeOfTask) {

        this.nameOfTask = nameOfTask;

        this.stateOfTask = stateOfTask;

        this.typeOfTask = typeOfTask;

    }

  

    createFocusSession(hoursFocussed, stateOfSession) {

        const arrStateOfSession = ["Active","Reviewing","Closed"];

        arrStateOfSession.sort();

        arrStateOfSession.find(function(e) { return e === stateOfSession });

        let dateOfSession = new Date(8.64e15).toString();

        let focusSession = {

            nameOfSession: this.nameOfTask,

            hoursFocussed: hoursFocussed,

            dateOfSession: dateOfSession,

            stateOfSession: stateOfSession

        };

        return `Focus Session: ${focusSession.nameOfSession}, Time Spent On Task: ${focusSession.hoursFocussed}, Date: ${dateOfSession}, State Of Session: ${focusSession.stateOfSession}`

    }

}

  

module.exports = taskTemplate;
```

