# Trabajo-Final-Programaci-n-Orientada-a-Objetos-
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Clase principal
public class SistemaHospital {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        GestorCitas gestor = new GestorCitas();
        int opcion;

        do {
            System.out.println("\n--- Sistema de Gestión de Citas Médicas ---");
            System.out.println("1. Registrar un médico");
            System.out.println("2. Registrar un usuario");
            System.out.println("3. Reservar una cita");
            System.out.println("4. Cancelar una cita");
            System.out.println("5. Mostrar citas");
            System.out.println("6. Salir");
            System.out.print("Seleccione una opción: ");
            opcion = scanner.nextInt();
            scanner.nextLine(); // Consumir la nueva línea

            switch (opcion) {
                case 1 -> {
                    System.out.print("ID del médico: ");
                    String idMedico = scanner.nextLine();
                    System.out.print("Nombre del médico: ");
                    String nombre = scanner.nextLine();
                    System.out.print("Especialidad: ");
                    String especialidad = scanner.nextLine();
                    gestor.registrarMedico(new Medico(idMedico, nombre, especialidad));
                }
                case 2 -> {
                    System.out.print("ID del usuario: ");
                    String idUsuario = scanner.nextLine();
                    System.out.print("Nombre del usuario: ");
                    String nombre = scanner.nextLine();
                    gestor.registrarUsuario(new Usuario(idUsuario, nombre));
                }
                case 3 -> {
                    System.out.print("ID del usuario: ");
                    String idUsuario = scanner.nextLine();
                    System.out.print("ID del médico: ");
                    String idMedico = scanner.nextLine();
                    System.out.print("Fecha de la cita (YYYY-MM-DD): ");
                    String fecha = scanner.nextLine();
                    System.out.print("Hora de la cita (HH:MM): ");
                    String hora = scanner.nextLine();
                    gestor.reservarCita(new Cita(fecha, hora, idUsuario, idMedico));
                }
                case 4 -> {
                    System.out.print("ID de la cita a cancelar: ");
                    String idCita = scanner.nextLine();
                    gestor.cancelarCita(idCita);
                }
                case 5 -> gestor.mostrarCitas();
                case 6 -> System.out.println("¡Hasta luego!");
                default -> System.out.println("Opción no válida.");
            }
        } while (opcion != 6);

        scanner.close();
    }
}

// Clase Usuario
class Usuario {
    private String id;
    private String nombre;

    public Usuario(String id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }

    public String getId() {
        return id;
    }

    public String getNombre() {
        return nombre;
    }
}

// Clase Medico
class Medico {
    private String id;
    private String nombre;
    private String especialidad;

    public Medico(String id, String nombre, String especialidad) {
        this.id = id;
        this.nombre = nombre;
        this.especialidad = especialidad;
    }

    public String getId() {
        return id;
    }

    public String getNombre() {
        return nombre;
    }

    public String getEspecialidad() {
        return especialidad;
    }
}

// Clase Cita
class Cita {
    private static int contador = 1;
    private String idCita;
    private String fecha;
    private String hora;
    private String idUsuario;
    private String idMedico;

    public Cita(String fecha, String hora, String idUsuario, String idMedico) {
        this.idCita = "C" + contador++;
        this.fecha = fecha;
        this.hora = hora;
        this.idUsuario = idUsuario;
        this.idMedico = idMedico;
    }

    public String getIdCita() {
        return idCita;
    }

    public String getFecha() {
        return fecha;
    }

    public String getHora() {
        return hora;
    }

    public String getIdUsuario() {
        return idUsuario;
    }

    public String getIdMedico() {
        return idMedico;
    }

    @Override
    public String toString() {
        return "Cita ID: " + idCita + ", Fecha: " + fecha + ", Hora: " + hora +
                ", Usuario ID: " + idUsuario + ", Médico ID: " + idMedico;
    }
}

// Clase GestorCitas
class GestorCitas {
    private List<Usuario> usuarios = new ArrayList<>();
    private List<Medico> medicos = new ArrayList<>();
    private List<Cita> citas = new ArrayList<>();

    public void registrarUsuario(Usuario usuario) {
        usuarios.add(usuario);
        System.out.println("Usuario registrado: " + usuario.getNombre());
    }

    public void registrarMedico(Medico medico) {
        medicos.add(medico);
        System.out.println("Médico registrado: " + medico.getNombre());
    }

    public void reservarCita(Cita cita) {
        boolean usuarioExiste = usuarios.stream().anyMatch(u -> u.getId().equals(cita.getIdUsuario()));
        boolean medicoExiste = medicos.stream().anyMatch(m -> m.getId().equals(cita.getIdMedico()));

        if (usuarioExiste && medicoExiste) {
            citas.add(cita);
            System.out.println("Cita reservada con éxito. ID de la cita: " + cita.getIdCita());
        } else {
            System.out.println("Usuario o médico no encontrado.");
        }
    }

    public void cancelarCita(String idCita) {
        citas.removeIf(c -> c.getIdCita().equals(idCita));
        System.out.println("Cita cancelada: " + idCita);
    }

    public void mostrarCitas() {
        if (citas.isEmpty()) {
            System.out.println("No hay citas registradas.");
        } else {
            citas.forEach(System.out::println);
        }
    }
}
