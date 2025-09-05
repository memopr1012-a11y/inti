import streamlit as st
import random

st.set_page_config(page_title="Ruleta de Intimidad (PG-13)", page_icon="🎡")

# Opciones base
default_items = [
    "Masaje de manos de 3 minutos",
    "Abrazo largo (60 segundos)",
    "Mirarse a los ojos 30 segundos",
    "Caricias en espalda por 2 minutos",
    "Contar un chiste favorito",
    "Planear una mini-cita",
    "Escribir un mini-agradecimiento",
]

# Estado
if "items" not in st.session_state:
    st.session_state.items = default_items.copy()

if "history" not in st.session_state:
    st.session_state.history = []

st.title("🎡 Ruleta de Intimidad (PG-13)")

# Mostrar opciones
with st.expander("📋 Opciones actuales"):
    for idx, it in enumerate(st.session_state.items, 1):
        st.write(f"{idx}. {it}")

# Añadir nueva opción
new_text = st.text_input("Añadir opción personalizada")
if st.button("➕ Añadir"):
    if new_text.strip():
        st.session_state.items.append(new_text.strip())
        st.success("Opción añadida ✅")

# Restablecer lista
if st.button("🔄 Restablecer lista"):
    st.session_state.items = default_items.copy()
    st.success("Lista restablecida ✅")

st.divider()

# Girar la ruleta
if st.button("🎲 Girar"):
    if st.session_state.items:
        resultado = random.choice(st.session_state.items)
        st.session_state.history.append(resultado)
        st.success(f"➡️ Resultado: **{resultado}**")
    else:
        st.error("No hay opciones disponibles")

# Historial
if st.session_state.history:
    st.subheader("📜 Historial de resultados")
    for idx, r in enumerate(reversed(st.session_state.history[-10:]), 1):
        st.write(f"{idx}. {r}")


