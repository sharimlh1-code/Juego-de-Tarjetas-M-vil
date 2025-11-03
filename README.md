# Juego-de-Tarjetas-M-vil
import random

tarjetas = {
    "fácil": [
        ("💙 Necesidad de afiliación", "¿Qué haces cuando quieres integrarte a un grupo nuevo? Explica qué necesidad psicológica hay detrás."),
        ("💛 Motivación de logro", "Cuenta una meta que hayas alcanzado gracias a tu esfuerzo."),
        ("💬 Percepción social", "¿Qué cosas notas primero cuando conoces a alguien nuevo?"),
    ],
    "medio": [
        ("❤️ Conducta altruista", "Di una acción altruista que hayas hecho o presenciado."),
        ("⚖️ Centro de control", "Si repruebas un examen, ¿a quién culparías? Explica si tu centro de control es interno o externo."),
        ("🧩 Atribución causal", "Tu amigo no llega a una cita. ¿Piensas que es irresponsable o que tuvo un problema? ¿Qué tipo de atribución estás haciendo?"),
        ("👁 Formación de impresiones", "Describe cómo cambia tu impresión de alguien cuando lo conoces mejor."),
    ],
    "difícil": [
        ("🔥 Agresividad", "Imita una situación de agresividad y luego muestra una manera asertiva de resolverla."),
        ("🎶 Acción musical", "Canta una frase relacionada con la motivación de logro."),
        ("🎭 Acción teatral", "Haz una mini escena de agresividad vs altruismo."),
        ("⚙️ Acción de rol", "Imita cómo reacciona alguien con centro de control externo."),
    ]
}

instrucciones = """
🎓 Juego de Tarjetas: Personalidad, Sociedad y Percepción Social

Instrucciones:
- Escribe el nombre de los jugadores cuando se les pida.
- Selecciona el nivel de dificultad para las tarjetas (fácil, medio o difícil).
- Por cada reto respondido, el grupo decide si la respuesta fue válida y suma 1 punto.
- Puedes dar puntos extra (bonus) según la creatividad o esfuerzo:
  - fácil: +1 punto máximo
  - medio: +1 o +2 puntos máximos
  - difícil: +1, +2 o +3 puntos máximos
- Escribe 't' para tomar una tarjeta, 'i' para ver las instrucciones, o 'salir' para terminar y ver el puntaje final.
"""

print(instrucciones)

# Cargar jugadores
num_jugadores = 0
while num_jugadores < 1:
    try:
        num_jugadores = int(input("¿Cuántos jugadores participan? "))
    except ValueError:
        print("Escribe un número válido.")

jugadores = []
for i in range(num_jugadores):
    nombre = input(f"Nombre del jugador {i+1}: ")
    jugadores.append({"nombre": nombre, "puntos": 0})

# Selección de dificultad
dificultad = ""
while dificultad not in tarjetas:
    dificultad = input("Selecciona dificultad (fácil/medio/difícil): ").strip().lower()
    if dificultad not in tarjetas:
        print("Opción inválida. Elige entre fácil, medio o difícil.")

turno = 0
while True:
    jugador = jugadores[turno % num_jugadores]
    print(f"\nTurno de {jugador['nombre']} (Puntos: {jugador['puntos']}). Dificultad: {dificultad.title()}")
    accion = input("[t: tomar tarjeta, i: instrucciones, salir]: ").strip().lower()
    if accion == 't':
        categoria, reto = random.choice(tarjetas[dificultad])
        print(f"\nTarjeta: {categoria}\nReto: {reto}\n")
        input("Presiona ENTER cuando termines tu respuesta o actuación...")

        valida = input("¿Respuesta válida? [s/n]: ").strip().lower()
        puntos_ganados = 0
        if valida == 's':
            puntos_ganados = 1
            # Bonus según dificultad elegida
            max_bonus = {"fácil": 1, "medio": 2, "difícil": 3}[dificultad]
            try:
                bonus = int(input(f"¿Cuántos puntos extra de bonificación das? [0-{max_bonus}]: ").strip())
            except ValueError:
                bonus = 0
            if 0 <= bonus <= max_bonus:
                puntos_ganados += bonus
                if bonus > 0:
                    print(f"¡{jugador['nombre']} recibe {bonus} punto(s) extra de bonificación!")
            else:
                print(f"Bonificación inválida, solo puedes dar entre 0 y {max_bonus} puntos extra.")
            jugador["puntos"] += puntos_ganados
            print(f"¡Puntos para {jugador['nombre']}: {puntos_ganados}! Total puntos: {jugador['puntos']}")
        else:
            print("Sin punto esta ronda.")
        turno += 1
    elif accion == 'i':
        print(instrucciones)
    elif accion == 'salir':
        print("\nPuntuación final:")
        for jug in jugadores:
            print(f"{jug['nombre']}: {jug['puntos']} puntos")
        print("¡Gracias por jugar!")
        break
    else:
        print("Opción no válida. Escribe 't', 'i' o 'salir'.")
