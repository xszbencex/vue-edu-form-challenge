# Form challenge
A feladat megoldásához szükséges kritériumok:

- A form összes mezője legyen kötelező.
- A form e-mail mezőjénél legyen megfelelő e-mail validáció.
- A név nem lehet kevesebb 5 karakternél, illetve nem lehet nagyobb 250 karakternél
- A jelszónak minimum 8 karakternek kell lennie
- A jelszó megerősítő input-ba beírt jelszónak meg kell egyeznie a felső jelszó mezővel
- A hibaüzeneteket elég pirossal az input alá tenni
- A "Sign up" gomb disabled legyen mindaddig, ameddig hibák vannak a form-ban (vagy üres)
- A "Sign up" gomb megnyomásánál csak simán írjuk ki az objektumot, amelyben a form elemei találhatóak

Első körben nem szükséges külső library-t használni hozzá (pl: Vuelidate).

A következő két koncepció hasznos lesz a feladat megoldása során:
- [Computed](https://vuejs.org/guide/essentials/computed.html)
- [Class Bindings](https://vuejs.org/guide/essentials/class-and-style.html)


To start this project you need to run:

```npm install```

and then

```npm run dev```
