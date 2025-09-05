import streamlit as st
import random

st.set_page_config(page_title="Ruleta de Intimidad (PG-13)", page_icon="🎡", layout="centered")

# ----- Datos base (PG-13, no explícitos) -----
default_items = [
    {"text": "Masaje de manos de 3 minutos", "cat": "pareja"},
    {"text": "Hacer una lista de 3 cosas que te encantan del otr@", "cat": "comunicacion"},
    {"text": "Abrazo largo (al menos 60 segundos)", "cat": "pareja"},
    {"text": "Bailar una canción completa", "cat": "pareja"},
    {"text": "Respiración sincronizada durante 1 minuto", "cat": "pareja"},
    {"text": "Auto-cuidado: estiramientos suaves por 2 minutos", "cat": "individual"},
    {"text": "Planear una mini-cita para esta semana", "cat": "comunicacion"},
    {"text": "Juego: describir un recuerdo bonito en 60s", "cat": "comunicacion"},
    {"text": "Mirarse a los ojos 30 segundos", "cat": "pareja"},
    {"text": "Caricias en espalda/hombros por 2 minutos", "cat": "pareja"},
    {"text": "Risa: contar un chiste o meme favorito", "cat": "comunicacion"},
    {"text": "Preparar una bebida para el otr@", "cat": "pareja"},
    {"text": "Afirmación: 'Me gusta de ti…' (completar)", "cat": "comunicacion"},
    {"text": "Caminata corta juntos (si es posible)", "cat": "bienestar"},
    {"text": "Tomarse una foto abrazados", "cat": "pareja"},
    {"text": "Auto-masaje de cuello 2 minutos", "cat": "individual"},
    {"text": "Escribir un mini-agradecimiento y leerlo", "cat": "comunicacion"},
    {"text": "Poner una canción y hacer 'air karaoke'", "cat": "bienestar"},
    {"text": "Cuddle/arrunche cómodo por 3 minutos", "cat": "pareja"},
    {"text": "Crear una palabra segura para hoy", "cat": "comunicacion"},
]

# Estado de la aplicación
if "items" not in st.session_state:
    st.session_state.items = default_items.copy()

if "history" not in st.session_state:
    st.session_state.history = []

st.title("🎡 Ruleta de Intimidad (PG-13)")
st.caption("Consentimiento y cuidado primero 💚 – Personaliza la ruleta según tus límites.")

# Filtro de categorías
cats = ["Todas", "pareja", "individual", "comunicacion", "bienestar"]
cat_filter = st.selectbox("Filtrar por categoría", cats)

# Lista filtrada
def filtered():
    if cat_filter == "Todas":
        return st.session_state.items
    return [it for it in st.session_state.items if it["cat"] == cat_filter]

# Mostrar opciones actuales
with st.expander("📋 Opciones actuales"):
    lista = filtered()
    if lista:
        for idx, it in enumerate(lista):
            st.write(f"{idx+1}. {it['text']} ({it['cat']})")
    else:
        st.info("No hay opciones para este filtro.")

# Añadir opción personalizada
st.subheader("➕ Añadir opción personalizada")
new_text = st.text_input("Texto de la opción")
new_cat = st.selectbox("Categoría", ["pareja", "individual", "comunicacion", "bienestar"])

if st.button("Añadir"):
    if new_text.strip():
        st.session_state.items.append({"text": new_text.strip(), "cat": new_cat})
        st.success("✅ Opción añadida a la ruleta")
    else:
        st.warning("Escribe una opción válida")

if st.button("🔄 Restablecer lista original"):
    st.session_state.items = default_items.copy()
    st.success("Lista restablecida ✅")

st.divider()

# Girar la ruleta
st.subheader("🎲 Girar la ruleta")
if st.button("Girar"):
    opciones = filtered()
    if opciones:
        resultado = random.choice(opciones)
        st.session_state.history.append(resultado)
        st.success(f"➡️ Resultado: **{resultado['text']}** (Categoría: {resultado['cat']})")
    else:
        st.error("No hay opciones disponibles con este filtro.")

# Historial
if st.session_state.history:
    st.subheader("📜 Historial de resultados")
    for idx, r in enumerate(reversed(st.session_state.history[-10:]), 1):
        st.write(f"{idx}. {r['text']} ({r['cat']})")

st.caption(\"\"\"Notas:\n• Respeto, cuidado y comunicación clara.\n• Paren en cualquier momento. Usa la palabra clave que acuerden.\n• Personaliza la lista según sus límites.\"\"\")
