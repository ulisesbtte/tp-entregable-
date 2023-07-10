# tp-entregable-
<!DOCTYPE html>
<html>

<head>
    <title>TP ENTREGABLE DISEÑO WEB</title>
    <style>
        td {
            width: 100px;
            height: 25px;
            border: 1px solid rgb(104, 31, 31);
        }
    </style>
    <script>
        const url = 'https://ca99-181-231-122-56.ngrok-free.app/student'

        window.onload = function () {
            getStudents()
        }

        function addStudent() {
            return new Promise(function (resolve, reject) {
                var request = new XMLHttpRequest()
                request.open('POST', url)
                request.setRequestHeader('Content-Type', 'application/json')
                var student = JSON.stringify({
                    'dni': document.getElementById('dni').value,
                    'lastName': document.getElementById('lastName').value,
                    'firstName': document.getElementById('firstName').value,
                    'email': document.getElementById('email').value,
                    'cohort': document.getElementById('cohort').value,
                    'status': document.getElementById('status').value,
                    'gender': document.getElementById('gender').value,
                    'address': document.getElementById('address').value,
                    'phone': document.getElementById('phone').value,
                })
                request.onload = function () {
                    if (request.status == 201) {
                        resolve(request.response)
                    } else {
                        reject(Error(request.statusText))
                    }
                }
                request.onerror = function () {
                    reject(Error('Error: unexpected network error.'))
                }
                request.send(student)
            })
        }

        function loadStudents() {
            return new Promise(function (resolve, reject) {
                var request = new XMLHttpRequest()
                request.open('GET', url + '/getAll')
                request.responseType = 'json'
                request.onload = function () {
                    if (request.status == 200) {
                        resolve(request.response)
                    } else {
                        reject(Error(request.statusText))
                    }
                }
                request.onerror = function () {
                    reject(Error('Error: unexpected network error.'))
                }
                request.send()
            })
        }

        function modifyStudent() {
            return new Promise(function (resolve, reject) {
                var request = new XMLHttpRequest()
                request.open('POST', url + `/${document.getElementById('id2').value}/update`)
                request.setRequestHeader('Content-Type', 'application/json')
                var student = JSON.stringify({
                    'dni': document.getElementById('dni2').value,
                    'lastName': document.getElementById('lastName2').value,
                    'firstName': document.getElementById('firstName2').value,
                    'email': document.getElementById('email2').value,
                    'cohort': document.getElementById('cohort2').value,
                    'status': document.getElementById('status2').value,
                    'gender': document.getElementById('gender2').value,
                    'address': document.getElementById('address2').value,
                    'phone': document.getElementById('phone2').value
                })
                request.onload = function () {
                    if (request.status == 200) {
                        resolve(request.response)
                    } else {
                        reject(Error(request.statusText))
                    }
                }
                request.onerror = function () {
                    reject(Error('Error: unexpected network error.'))
                }
                request.send(student)
            })
        }

        function removeStudent(Id) {
            return new Promise(function (resolve, reject) {
                var request = new XMLHttpRequest()
                request.open('POST', url + `/${Id}/delete`)
                request.setRequestHeader('Content-Type', 'application/json')
                request.onload = function () {
                    if (request.status == 200) {
                        resolve(request.response)
                    } else {
                        reject(Error(request.statusText))
                    }
                }
                request.onerror = function () {
                    reject(Error('Error: unexpected network error.'))
                }
                request.send()
            })
        }

        function getStudents() {
            loadStudents().then(response => {
                var tbody = document.getElementById('tabla')
                tbody.innerHTML = ''
                let total = 0
                response.forEach(e => {
                    total++
                    var row = tbody.insertRow()
                    var Id = row.insertCell()
                    Id.innerHTML = e.id
                    var dni = row.insertCell()
                    dni.innerHTML = e.dni
                    var lastName = row.insertCell()
                    lastName.innerHTML = e.lastName
                    var firstName = row.insertCell()
                    firstName.innerHTML = e.firstName
                    var email = row.insertCell()
                    email.innerHTML = e.email

                    var student = JSON.stringify({
                        'id': e.id,
                        'dni': e.dni,
                        'lastName': e.lastName,
                        'firstName': e.firstName,
                        'cohort': e.cohort,
                        'address': e.address,
                        'status': e.status,
                        'gender': e.gender,
                        'phone': e.phone,
                    })

                    document.getElementById('totalEstudiantes').innerHTML = `Cantidad total de estudiantes: ${total}`
                    var view = row.insertCell()
                    view.innerHTML = `<button onclick='viewStudent(${student})'>Ver</button>`
                    var del = row.insertCell()
                    del.innerHTML = `<button onclick='deleteStudent(${e.id})'>Eliminar</button>`
                })

            }).catch(reason => {
                console.error(reason)
            })
        }

        function saveStudent() {
            addStudent().then(() => {
                document.getElementById('dni').value = ''
                document.getElementById('lastName').value = ''
                document.getElementById('firstName').value = ''
                document.getElementById('email').value = ''
                document.getElementById('cohort').value = ''
                document.getElementById('status').value = ''
                document.getElementById('gender').value = ''
                document.getElementById('address').value = ''
                document.getElementById('phone').value = ''

                getStudents()
            }).catch(reason => {
                console.error(reason)
            })

        }

        function viewStudent(student) {
            document.getElementById('id2').value = student.id
            document.getElementById('dni2').value = student.dni
            document.getElementById('firstName2').value = student.firstName
            document.getElementById('lastName2').value = student.lastName
            document.getElementById('email2').value = student.email
            document.getElementById('cohort2').value = student.cohort
            document.getElementById('status2').value = student.status
            document.getElementById('address2').value = student.address
            document.getElementById('gender2').value = student.gender
        }

        function updateStudent() {
            modifyStudent().then(() => {
                document.getElementById('id2').value = ''
                document.getElementById('dni2').value = ''
                document.getElementById('lastName2').value = ''
                document.getElementById('firstName2').value = ''
                document.getElementById('email2').value = ''
                document.getElementById('cohort2').value = ''
                document.getElementById('status2').value = ''
                document.getElementById('gender2').value = ''
                document.getElementById('address2').value = ''
                document.getElementById('phone2').value = ''
                getStudents()
            }).catch(reason => {
                console.error(reason)
            })
        }

        function deleteStudent(id) {
            removeStudent(id).then(() => {
                getStudents()
            }).catch(reason => {
                console.error(reason)
            })
        }


    </script>
</head>

<body>
    <h3>Crear estudiantes</h3>
    <label for="dni">DNI: </label>
    <input type="text" Id="dni">
    <br>
    <label for="firstName">Name: </label>
    <input type="text" Id="firstName">
    <br>
    <label for="lastName">Lastname: </label>
    <input type="text" Id="lastName">
    <br>
    <label for="email">Email: </label>
    <input type="text" Id="email">
    <br>
    <label for="cohort">Cohort: </label>
    <input type="text" Id="cohort">
    <br>
    <label for="phone">Phone: </label>
    <input type="text" Id="phone">
    <br>
    <label for="status">Status: </label>
    <input type="text" Id="status">
    <br>
    <label for="address">Address: </label>
    <input type="text" Id="address">
    <br>
    <label for="gender">Gender: </label>
    <input type="text" Id="gender">
    <br>
    <button onclick="saveStudent()">Crear estudiante</button>



    <h3>Modificar estudiantes</h3>
    <label for="id2">Id: </label>
    <input type="text" readonly Id="id2">
    <br>
    <label for="dni2">DNI: </label>
    <input type="text" Id="dni2">
    <br>
    <label for="firstName2">Name: </label>
    <input type="text" Id="firstName2">
    <br>
    <label for="lastName2">Lastname: </label>
    <input type="text" Id="lastName2">
    <br>
    <label for="email2">Email: </label>
    <input type="text" Id="email2">
    <br>
    <label for="cohort2">Cohort: </label>
    <input type="text" Id="cohort2">
    <br>
    <label for="phone2">Phone: </label>
    <input type="text" Id="phone2">
    <br>
    <label for="status2">Status: </label>
    <input type="text" Id="status2">
    <br>
    <label for="address2">Address: </label>
    <input type="text" Id="address2">
    <br>
    <label for="gender2">Gender: </label>
    <input type="text" Id="gender2">
    <br>
    <button onclick="updateStudent()">Modificar estudiante</button>


    <h3 id="totalEstudiantes"><h3>

    <table>
        <thead>
            <tr>
                <th>ID</th>
                <th>DNI</th>
                <th>APELLIDO</th>
                <th>NOMBRE</th>
                <th>EMAIL</th>
            </tr>
        </thead>
        <tbody Id="tabla"></tbody>
    </table>
</body>

</html>
