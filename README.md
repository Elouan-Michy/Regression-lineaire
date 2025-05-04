import streamlit as st
import pandas as pd
import numpy as np
from st_aggrid import AgGrid, GridOptionsBuilder, GridUpdateMode


# Données aléatoires
n_rows = 10
df = pd.DataFrame({
    'Nom': [f'Pers{i}' for i in range(n_rows)],
    'Âge': np.random.randint(18, 60, size=n_rows),
    'Score': np.random.uniform(0, 100, size=n_rows).round(2),
    'Année': np.random.choice([2000 + i for i in range(20)], size=n_rows)
})

# Choisir une ligne arbitrairement (ou utiliser une logique ailleurs)
i = 0
ligne = df.iloc[i]

# Layout à 3 colonnes
col1, col2, col3 = st.columns([2, 1.5, 1])

X = "Chocolat"

with col1:
    st.markdown(
        "<h3 style='text-align: center;'>Type :</h3>",
        unsafe_allow_html=True
    )
    st.markdown(
        f"""
        <div style='
            background-color: #5fb7cf;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 2px 2px 8px rgba(0,0,0,0.1);
            text-align: center;
            font-family: "Segoe UI", sans-serif;
        '>
            <h1 style='margin: 0; color: #444;'>{X}</h1>
        </div>
        """,
        unsafe_allow_html=True
    )

with col2:
    st.markdown(
        "<h3 style='text-align: center;'>Années :</h3>",
        unsafe_allow_html=True
    )
    st.markdown(
        f"""
        <div style='
            border: 1px solid #5fb7cf;
            border-radius: 10px;
            padding: 12px;
            height: 160px;
            overflow-y: auto;
            background: linear-gradient(to bottom, #5fb7cf, #3c339a);
            box-shadow: 2px 2px 6px rgba(0,0,0,0.1);
            font-family: "Segoe UI", sans-serif;
            font-size: 15px;
            color: #333;
        '>
            {"".join(f"<div style='margin-bottom: 6px; padding: 4px 8px; border-radius: 4px; background-color: #ffffff;'>{2*a}</div>" for a in range(5))}
        </div>
        """,
        unsafe_allow_html=True
    )

    
with col3:
    st.markdown(f"<h2 style='text-align:right'>{ligne['Année']}</h2>", unsafe_allow_html=True)
    
    

df = pd.DataFrame({
    'Nom': ['Alice', 'Bob', 'Charlie', 'David'],
    'Âge': [25, 30, 35, 40],
    'Ville': ['Paris', 'Lyon', 'Marseille', 'Nice']
})

col11, col22 = st.columns([1, 1])

with col11:
    
    # Configuration du tableau
    gb = GridOptionsBuilder.from_dataframe(df)
    gb.configure_selection(selection_mode='single', use_checkbox=False)
    gb.configure_grid_options(
        suppress_row_click_selection=False,
        domLayout='autoHeight',
        pagination=True
    )
    
    # Personnaliser le style du tableau
    grid_options = gb.build()
    
    # Personnalisation de l'apparence du tableau
    st.subheader("Tableau interactif (clique sur une ligne) :")
    grid_response = AgGrid(
        df,
        gridOptions=grid_options,
        update_mode=GridUpdateMode.SELECTION_CHANGED,
        height=300,
        width='100%',
        fit_columns_on_grid_load=True,
        theme='streamlit',  # ou 'light', 'dark', 'blue'
    )
    
    # Récupérer la ligne sélectionnée
    selected = grid_response["selected_rows"]
    
with col22:

    if selected is not None and len(selected) > 0:
        personne = selected
        st.write("Tu as sélectionné :", personne)
    else:
        st.info("Aucune ligne sélectionnée.")
