## DOCUMENTACION FRONTEND  
### Proyecto ***"Farmacia"***

El objetivo principal de este proyecto es el de mostrar un registro de las diferentes framacias disponibles dentro de la apliacaion
junto con los datos que identifican a cada una, tales como el nombre de esta, su correo de contacto al igual que su numero telefonico, 
entre otros. Todo esto se realizó mediante un conjunto de Netbeans conectado a una base de datos en MySQL   

![Imagen desde Google Drive](https://drive.google.com/uc?export=view&id=1OGGkBCdPrywXzztyAyqIcNsxoo0_0G_f)  

Para este pequeño proyecto se cuenta con una pequeña interfaz la cual cuenta con tan solo cuatro botones principales que nos permitiran
realizar las acciones necesarias para el uso de esta interfaz: ***Agregar**, **Eliminar**, **Editar*** y por ultimo ***Buscar***. Tambien
cuenta con un textfield el cual el usuario escribirá el nombre de la Farmacia que quiera encontrar complementado al boton de ***Buscar*** para
realizar la función esperada de busqueda. Y por ultimo contamos con una tabla donde podemos ver los registros mandados desde nuestra base de datos a 
la interfaz de usuario.  

### \> ***Obtencion de datos en la tabla***

>La tabla imprime los valores de la base de datos mediante la siguiente función:

```Java
public static List<Laboratorio> getLaboratorios() {
        List<Laboratorio> lista = new ArrayList<>();
        MySQLConnect conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try{
            conn = new MySQLConnect();
            String query = "SELECT * FROM Laboratorio;";
            if (conn != null) {
                ps = conn.connection().prepareStatement(query);
                rs = ps.executeQuery();
                if (rs != null) {
                    while (rs.next()) {
                        Integer idLaboratorio = rs.getInt("idLaboratorio");
                        String nombre = rs.getString("nombre");
                        String correoElectronico = rs.getString("correoElectronico");
                        Integer idDireccion = rs.getInt("idDireccion");
                        lista.add(new Laboratorio(idLaboratorio, nombre, correoElectronico, idDireccion));
                    }
                }else{
                    System.out.println("ResultSet null...");
                }
                rs.close();
                ps.close();
                conn.close();
            }else{
                System.out.println("Error de conexión");
            }
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        } finally {
            try {
                if (rs != null) {
                    rs.close();
                }
                if (ps != null) {
                    ps.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return lista;
    }
```

### \> ***Boton de Registrar***  ![Imagen 1](https://drive.google.com/uc?export=view&id=1AiyUdvPpbSRrJSDL_COTbT3KCgrpVQwS)  

>Al presionar este boton se ejecutará la siguiente funcion del codigo:

```Java
public static boolean registrar(Laboratorio laboratorio){
        MySQLConnect conn = null;
        PreparedStatement ps = null;
        try {
            conn = new MySQLConnect();
            String query = "INSERT INTO Laboratorio (idLaboratorio, nombre, correoElectronico, idDireccion) "
                                + "VALUES(NULL, ?, ?, ?);";
            if (conn != null) {
                ps = conn.connection().prepareStatement(query);
                ps.setString(1,laboratorio.getNombre());
                ps.setString(2,laboratorio.getCorreoElectronico());
                ps.setInt(3,laboratorio.getIdDireccion());
                ps.executeUpdate();
                ps.close();
                conn.close();
                return true;
            } else {
                System.out.println("Error de conexión");
            }
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        } finally {
            try {
                if (ps != null) {
                    ps.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return false;
    }
```

### \> ***Boton de Editar***  ![Imagen 2](https://drive.google.com/uc?export=view&id=1vhAhizpBczv8sW-LaqplzC0McZZSDcvu)    

>Igualmente al hacer click en el boton, se abrirá una pantalla pequeña en la que podremos observar los datos de la farmacia de una forma mas detallada.  

![Imagen desde Google Drive](https://drive.google.com/uc?export=view&id=1XDP-_pDaO7m7XAHoRq8CJQEl3O3i0FPo)

>Aqui podremos editar los valores establecidos de la misma, igualmente contamos con dos botones, al presionar el boton de aceptar se realizará la siguiente función:

```Java
public static boolean actualizar(Laboratorio laboratorio){
        MySQLConnect conn = null;
        PreparedStatement ps = null;
        try {
            conn = new MySQLConnect();
            String query = "UPDATE Laboratorio SET nombre = ?, correoElectronico = ?, idDireccion = ? "
                                + "WHERE idLaboratorio = ?;";
            if (conn != null) {
                ps = conn.connection().prepareStatement(query);
                ps.setString(1,laboratorio.getNombre());
                ps.setString(2, laboratorio.getCorreoElectronico());
                ps.setInt(3, laboratorio.getIdDireccion());
                ps.setInt(4, laboratorio.getIdLaboratorio());
                ps.executeUpdate();
                ps.close();
                conn.close();
                return true;
            } else {
                System.out.println("Error de conexión");
            }
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        } finally {
            try {
                if (ps != null) {
                    ps.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return false;
    }
```

### \> ***Boton de Eliminar***  ![Imagen 3](https://drive.google.com/uc?export=view&id=1od9lFqzM8tIVdkY2lZn8Ob585gUAs4Rp)  

>Al presionar el siguiente boton se eliminará el registro seleccionado de la tabla, y por consecuente, de la base de datos:  

```Java
public static boolean eliminar(Integer idLaboratorio) {
    MySQLConnect conn = null;
    PreparedStatement ps = null;
    try {
        conn = new MySQLConnect();
        conn.connection().setAutoCommit(false); // Inicia la transacción
        
        // Elimina los registros relacionados en la tabla Telefono
        String query1 = "DELETE FROM Telefono WHERE idLaboratorio = ?;";
        ps = conn.connection().prepareStatement(query1);
        ps.setInt(1, idLaboratorio);
        ps.executeUpdate();
        ps.close();

        // Elimina el registro en la tabla Laboratorio
        String query2 = "DELETE FROM Laboratorio WHERE idLaboratorio = ?;";
        ps = conn.connection().prepareStatement(query2);
        ps.setInt(1, idLaboratorio);
        ps.executeUpdate();
        ps.close();

        conn.connection().commit(); // Confirma la transacción
        return true; // Éxito en la operación
    } catch (SQLException e) {
        try {
            if (conn != null) conn.connection().rollback(); // Revertir transacción
        } catch (SQLException rollbackEx) {
            rollbackEx.printStackTrace();
        }
        System.out.println("Error al eliminar: " + e.getMessage());
        throw new RuntimeException(e); // Relanzar la excepción
    } finally {
        try {
            if (ps != null) ps.close();
            if (conn != null) conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### \> ***Busqueda***  ![Imagen desde Google Drive](https://drive.google.com/uc?export=view&id=1yDAGgjAFg_AVguieKjdyD039hsPkNSBe)  

> Se realiza una busqueda por nombre de la farmacia deseada, dentro del textfield se ingresa el nombre y posteriormente se hace click en el boton de buscar para que se realiza la siguiente función:

```Java
public static List<Laboratorio> buscarPorNombre(String nombrebusqueda) {
        List<Laboratorio> lista = new ArrayList<>();
        MySQLConnect conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try{
            conn = new MySQLConnect();
            String query = "SELECT * FROM Laboratorio WHERE nombre LIKE ?;";
            if (conn != null) {
                ps = conn.connection().prepareStatement(query);
                ps.setString(1, "%" + nombrebusqueda + "%");
                rs = ps.executeQuery();
                if (rs != null) {
                    while (rs.next()) {
                        Integer idLaboratorio = rs.getInt("idLaboratorio");
                        String nombre = rs.getString("nombre");
                        String correoElectronico = rs.getString("correoElectronico");
                        Integer idDireccion = rs.getInt("idDireccion");
                        lista.add(new Laboratorio(idLaboratorio, nombre, correoElectronico, idDireccion));
                    }
                }else{
                    System.out.println("ResultSet null...");
                }
                rs.close();
                ps.close();
                conn.close();
            }else{
                System.out.println("Error de conexión");
            }
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        } finally {
            try {
                if (rs != null) {
                    rs.close();
                }
                if (ps != null) {
                    ps.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return lista;
    }
```


