#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Declaración anticipada de Usuario
class Usuario;

class Publicacion {
private:
    int id;
    string fecha;
    string contenido;
    Usuario* usuario;

public:
    Publicacion(Usuario* usuario, string fecha, string contenido);
    void mostrarPublicacion();
};

class Usuario {
private:
    int id;
    string nombre;
    int edad;
    string nacionalidad;
    vector<Usuario*> amigos;
    vector<Publicacion*> publicaciones;

public:
    Usuario(string nombre, int edad, string nacionalidad);
    int getId();
    string getNombre();
    void mostrar();
    void mostrarAmigos();
    void mostrarPublicaciones();
    void agregarAmigo(Usuario* nuevoAmigo);
    void crearPublicacion(string fecha, string contenido);
};

class RedSocial {
private:
    string nombre;
    vector<Usuario*> usuarios;
    vector<Publicacion*> publicaciones;

public:
    RedSocial(string nombre);
    RedSocial(string nombre, vector<Usuario*> usuarios);
    RedSocial(string nombre, vector<Usuario*> usuarios, vector<Publicacion*> publicaciones);
    void agregarUsuario(Usuario* usuario);
    void mostrarUsuarios();
    void mostrarPublicaciones();
    Usuario* getUsuario(int id);
};

// Implementación de los métodos de la clase Publicacion
Publicacion::Publicacion(Usuario* usuario, string fecha, string contenido) {
    static int nextId = 1;
    this->id = nextId++;
    this->fecha = fecha;
    this->contenido = contenido;
    this->usuario = usuario;
}

void Publicacion::mostrarPublicacion() {
    cout << "Fecha: " << fecha << endl;
    cout << "Contenido: " << contenido << endl;
    cout << "Usuario: " << usuario->getNombre() << endl;
}

// Implementación de los métodos de la clase Usuario
Usuario::Usuario(string nombre, int edad, string nacionalidad) {
    static int nextId = 1;
    this->id = nextId++;
    this->nombre = nombre;
    this->edad = edad;
    this->nacionalidad = nacionalidad;
}

int Usuario::getId() {
    return id;
}

string Usuario::getNombre() {
    return nombre;
}

void Usuario::mostrar() {
    cout << "ID: " << id << endl;
    cout << "Nombre: " << nombre << endl;
    cout << "Edad: " << edad << endl;
    cout << "Nacionalidad: " << nacionalidad << endl;
}

void Usuario::mostrarAmigos() {
    cout << "Amigos de " << nombre << ":" << endl;
    for (Usuario* amigo : amigos) {
        cout << amigo->getNombre() << endl;
    }
}

void Usuario::mostrarPublicaciones() {
    cout << "Publicaciones de " << nombre << ":" << endl;
    for (Publicacion* publicacion : publicaciones) {
        publicacion->mostrarPublicacion();
    }
}

void Usuario::agregarAmigo(Usuario* nuevoAmigo) {
    amigos.push_back(nuevoAmigo);
    nuevoAmigo->amigos.push_back(this);
}

void Usuario::crearPublicacion(string fecha, string contenido) {
    Publicacion* nuevaPublicacion = new Publicacion(this, fecha, contenido);
    publicaciones.push_back(nuevaPublicacion);
}

// Implementación de los métodos de la clase RedSocial
RedSocial::RedSocial(string nombre) {
    this->nombre = nombre;
}

RedSocial::RedSocial(string nombre, vector<Usuario*> usuarios) : RedSocial(nombre) {
    this->usuarios = usuarios;
}

RedSocial::RedSocial(string nombre, vector<Usuario*> usuarios, vector<Publicacion*> publicaciones) : RedSocial(nombre, usuarios) {
    this->publicaciones = publicaciones;
}

void RedSocial::agregarUsuario(Usuario* usuario) {
    usuarios.push_back(usuario);
}

void RedSocial::mostrarUsuarios() {
    cout << "Usuarios de la red social " << nombre << ":" << endl;
    for (Usuario* usuario : usuarios) {
        usuario->mostrar();
    }
}

void RedSocial::mostrarPublicaciones() {
    cout << "Publicaciones en la red social " << nombre << ":" << endl;
    for (Publicacion* publicacion : publicaciones) {
        publicacion->mostrarPublicacion();
    }
}

Usuario* RedSocial::getUsuario(int id) {
    for (Usuario* usuario : usuarios) {
        if (usuario->getId() == id) {
            return usuario;
        }
    }
    cout << "No existe un usuario con el ID proporcionado." << endl;
    return nullptr;
}

int main() {
    // Ejemplo de uso
    RedSocial miRedSocial("Mi Red Social");

    Usuario* usuario1 = new Usuario("Usuario1", 25, "Mexico");
    Usuario* usuario2 = new Usuario("Usuario2", 30, "España");

    miRedSocial.agregarUsuario(usuario1);
    miRedSocial.agregarUsuario(usuario2);

    usuario1->agregarAmigo(usuario2);
    usuario1->crearPublicacion("2024-03-29", "¡Hola mundo!");

    miRedSocial.mostrarUsuarios();
    miRedSocial.mostrarPublicaciones();

    delete usuario1;
    delete usuario2;

    return 0;
}
