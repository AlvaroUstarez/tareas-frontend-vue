<template>
    <div>
        <h1 class="display-4 text-center">Listado de Propiedades</h1>
        <hr>
        <div class="row container fldsmdfr">
            <div class="col-9">
                <input type="text" v-model="tarea.id" class="form-control mb-3" placeholder="Id">

                <input type="text" v-model="tarea.tipoPropiedad" class="form-control mb-3" placeholder="Tipo propiedad">

                <input type="text" v-model="tarea.tipoOperacion" class="form-control mb-3" placeholder="Tipo operación">

                <input type="text" v-model="tarea.descripcion" class="form-control mb-3" placeholder="Descripción">

                <input type="text" v-model="tarea.ambientes" class="form-control mb-3"
                    placeholder="Cantidad de ambientes">

                <input type="text" v-model="tarea.m2" class="form-control mb-3" placeholder="Metros cuadrados">

                <input type="text" v-model="tarea.antiguedad" class="form-control mb-3"
                    placeholder="Agregar antigüedad">

                <div class="input-group-append">
                    <button v-on:click="agregarTarea()" class="btn btn-success">
                        Guardar
                    </button>
                </div>
                <br>
                <table class="table">
                    <thead>
                        <tr>
                            <th scope="col">Id</th>
                            <th scope="col">Tipo Propiedad</th>
                            <th scope="col">Tipo Operación</th>
                            <th scope="col">Descripción</th>
                            <th scope="col">Ambientes</th>
                            <th scope="col">Metros cuadrados</th>
                            <th scope="col">Antigüedad</th>
                            <th scope="col">Acciónes</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(tarea, index) of listTareas" :key="index">
                            <td>{{ tarea.id }}</td>
                            <td>{{ tarea.tipoPropiedad }}</td>
                            <td>{{ tarea.tipoOperacion }}</td>
                            <td>{{ tarea.descripcion }}</td>
                            <td>{{ tarea.ambientes }}</td>
                            <td>{{ tarea.m2 }}</td>
                            <td>{{ tarea.antiguedad }}</td>
                            <td>
                                <button v-on:click="agregarTarea()" class="btn btn-primary btn-sm">
                                    ver
                                </button>
                            </td>
                            <td>
                                <button v-on:click="editarTarea(tarea, tarea.id)" class="btn btn-success btn-sm">
                                    editar
                                </button>
                            </td>
                            <td>
                                <button v-on:click="eliminarTarea(tarea.id)" class="btn btn-danger btn-sm">
                                    eliminar
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: 'TaRea',
    data() {
        return {
            tarea: {
                id: null,
                tipoPropiedad: '',
                tipoOperacion: '',
                descripcion: '',
                ambientes: null,
                m2: null,
                antiguedad: null,
                coordenadas: {
                    latitud: null,
                    longitud: null
                },
                // imagenes:[]
            },
            listTareas: []
        }
    },
    methods: {
        agregarTarea() {
            const nuevaTarea = {
                id: this.tarea.id,
                tipoPropiedad: this.tarea.tipoPropiedad,
                tipoOperacion: this.tarea.tipoOperacion,
                descripcion: this.tarea.descripcion,
                ambientes: this.tarea.ambientes,
                m2: this.tarea.m2,
                antiguedad: this.tarea.antiguedad
            }

            const tareaEncontradaIndex = this.listTareas.findIndex(tarea => tarea.id === nuevaTarea.id);

            // Si se encontró una tarea con el mismo id, actualizar sus propiedades en lugar de agregar una nueva tarea
            if (tareaEncontradaIndex !== -1) {
                // this.listTareas[tareaEncontradaIndex] = nuevaTarea; // Actualizar la tarea existente con los nuevos datos
                axios.put("https://localhost:44388/api/Tarea/" + this.tarea.id, nuevaTarea).then(response => {
                    console.log('response editar tarea', response);
                    this.obtenerTareas();
                }).catch(error => {
                    console.error(error);
                })
            } else {
                // Si no se encontró ninguna tarea con el mismo id, agregar la nueva tarea a la lista
                // this.listTareas.push(nuevaTarea);
                axios.post("https://localhost:44388/api/Tarea/", nuevaTarea).then(response => {
                    console.log('response nueva tarea', response);
                    this.obtenerTareas();
                }).catch(error => {
                    console.error(error);
                })
            }

            this.tarea.id = null;
            this.tarea.tipoPropiedad = '';
            this.tarea.tipoOperacion = '';
            this.tarea.descripcion = "",
                this.tarea.ambientes = "",
                this.tarea.m2 = "",
                this.tarea.antiguedad = ""
            // console.log(nuevaTarea);
        },
        eliminarTarea(id) {
            // this.listTareas.splice(id, 1)
            axios.delete("https://localhost:44388/api/Tarea/" + id).then(response => {
                console.log('eliminar tarea response', response);
                this.obtenerTareas();
            }).catch(error => {
                console.error(error);
            })
        },
        editarTarea(tarea, id) {
            console.log('Tarea', tarea.tipoPropiedad);
            console.log('index', tarea.id);

            // Busca la tarea en la listaTareas que tenga el id correspondiente
            const tareaEditar = this.listTareas.find(tarea => tarea.id === id);

            // Verifica si se encontró la tarea con el id especificado
            if (tareaEditar) {
                // Actualiza las propiedades de la tarea encontrada

                console.log();
                console.log(tareaEditar.tipoPropiedad);

                this.tarea.id = tareaEditar.id;
                this.tarea.tipoPropiedad = tareaEditar.tipoPropiedad;
                this.tarea.tipoOperacion = tareaEditar.tipoOperacion;
                this.tarea.descripcion = tareaEditar.descripcion;
                this.tarea.ambientes = tareaEditar.ambientes;
                this.tarea.m2 = tareaEditar.m2;
                this.tarea.antiguedad = tareaEditar.antiguedad;

                // tareaEditar.tipoPropiedad = this.tarea.tipoPropiedad;
                // tareaEditar.tipoOperacion = this.tarea.tipoOperacion;
                // tareaEditar.descripcion = this.tarea.descripcion;
                // tareaEditar.ambientes = this.tarea.ambientes;
                // tareaEditar.m2 = this.tarea.m2;
                // tareaEditar.antiguedad = this.tarea.antiguedad;

                // Restablece las propiedades de this.tarea a sus valores predeterminados
                // this.tarea.tipoPropiedad = '';
                // this.tarea.tipoOperacion = '';
                // this.tarea.descripcion = '';
                // this.tarea.ambientes = '';
                // this.tarea.m2 = '';
                // this.tarea.antiguedad = '';
            } else {
                console.error('No se encontró ninguna tarea con el id especificado:', id);
            }

            // this.listTareas[id].tipoPropiedad = tipoPropiedad;
            // this.listTareas[id].tipoOperacion = tipoOperacion;
            // this.listTareas[id].descripcion = descripcion;
            // this.listTareas[id].ambientes = ambientes;
            // this.listTareas[id].m2 = m2;
            // this.listTareas[id].antiguedad = antiguedad;

            // tipoPropiedad = this.listTareas[id].tipoPropiedad; 





        },
        obtenerTareas() {
            // return this.listTareas;
            axios.get("https://localhost:44388/api/Tarea").then(response => {
                console.debug(response);
                this.listTareas = response.data;
            })

        }
        
    },
    created: function () {
        const nuevaTarea = {
            id: 1,
            tipoPropiedad: "Quinta",
            tipoOperacion: "Transacción",
            descripcion: "Quinta de olivos",
            ambientes: "25",
            m2: "5000",
            antiguedad: "100"
        }
        this.listTareas.push(nuevaTarea);
        this.obtenerTareas();
    }
}
</script>

<style scoped>
.cursor {
    cursor: pointer;
}

.fldsmdfr {
    justify-content: center;
}
</style>