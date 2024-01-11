Aqui podemos ver que no solo se puede acceder por correo y por Google si no tambien por facebook 
![image](https://github.com/ale20sha/Facebook/assets/153215838/9e42284a-873c-483e-ac93-a26625dd1655)


Damos click 
nos manda a la pagina 
![image](https://github.com/ale20sha/Facebook/assets/153215838/72cd415e-3bd4-4091-9888-30e129f36971)

y final mente metemos el correo 
 ![image](https://github.com/ale20sha/Facebook/assets/153215838/07580c2f-0876-4971-bf64-6797b6945578)

 ![image](https://github.com/ale20sha/Facebook/assets/153215838/997ae1e7-c6a2-4778-8977-02a8b30f47cc)
 

Aqui esta mi codigo de componente Facebook.js 
import React, { useState, useEffect } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInWithPopup, FacebookAuthProvider, onAuthStateChanged } from 'firebase/auth';

const firebaseConfig = {
  apiKey: "AIzaSyArsTtN2sl9f5o5rn76AIHo90Hi7MXzV0M",
  authDomain: "login15601-15946.firebaseapp.com",
  projectId: "login15601-15946",
  storageBucket: "login15601-15946.appspot.com",
  messagingSenderId: "947415247332",
  appId: "1:947415247332:web:ef10a8ca74687b82153ff1"
};

const app = initializeApp(firebaseConfig);

const Facebook = () => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const auth = getAuth();

    const unsubscribe = onAuthStateChanged(auth, (authUser) => {
      if (authUser) {
        // Usuario inició sesión
        setUser(authUser);
      } else {
        // Usuario cerró sesión
        setUser(null);
      }
    });

    return () => {
      // Limpiar efecto cuando el componente se desmonta
      unsubscribe();
    };
  }, []);

  const handleFacebookLogin = async () => {
    const auth = getAuth();
    const provider = new FacebookAuthProvider();

    try {
      const result = await signInWithPopup(auth, provider);
      const user = result.user;
      setUser(user);
    } catch (error) {
      console.error(error.message);
    }
  };

  const handleLogout = async () => {
    const auth = getAuth();
    try {
      await auth.signOut();
      setUser(null);
    } catch (error) {
      console.error(error.message);
    }
  };

  return (
    <div>
      {user ? (
        <div>
          <p>Hola, {user.displayName}!</p>
          <button onClick={handleLogout}>Cerrar sesión</button>
        </div>
      ) : (
        <div>
          <p>Inicia sesión con Facebook</p>
          <button onClick={handleFacebookLogin}>Iniciar sesión</button>
        </div>
      )}
    </div>
  );
};

export default Facebook;



