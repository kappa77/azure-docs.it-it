---
title: 'Esercitazione: Integrazione di Azure Active Directory con Zoom | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Zoom.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 641fe5439e320208d41969b9563293257648d488
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2019
ms.locfileid: "65895953"
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Esercitazione: Integrazione di Azure Active Directory con Zoom

Questa esercitazione descrive come integrare Zoom con Azure Active Directory (Azure AD).
L'integrazione di Zoom con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a Zoom.
* È possibile abilitare gli utenti per l'accesso automatico (Single Sign-On) a Zoom con gli account Azure AD personali.
* È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Zoom, sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si dispone di un ambiente Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/)
* Sottoscrizione di Zoom abilitata per l'accesso Single Sign-On

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* Zoom supporta l'accesso SSO avviato da **SP**

## <a name="adding-zoom-from-the-gallery"></a>Aggiunta di Zoom dalla raccolta

Per configurare l'integrazione di Zoom in Azure AD, è necessario aggiungere Zoom dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere Zoom dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory](common/select-azuread.png)

2. Passare ad **Applicazioni aziendali** e quindi selezionare l'opzione **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali](common/enterprise-applications.png)

3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione](common/add-new-app.png)

4. Nella casella di ricerca digitare **Zoom**, selezionare **Zoom** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

     ![Zoom nell'elenco risultati](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Zoom usando un utente di test di nome **Britta Simon**.
Per il corretto funzionamento dell'accesso Single Sign-On, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Zoom.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Zoom, è necessario completare le procedure di base seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
2. **[Configurare l'accesso Single Sign-On di Zoom](#configure-zoom-single-sign-on)**: per configurare le impostazioni di Single Sign-On sul lato applicazione.
3. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Creare l'utente di test di Zoom](#create-zoom-test-user)**: per avere una controparte di Britta Simon in Zoom collegata alla rappresentazione dell'utente in Azure AD.
6. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure.

Per configurare l'accesso Single Sign-On di Azure AD con Zoom, seguire questa procedura:

1. Nella pagina di integrazione dell'applicazione **Zoom** del [portale di Azure](https://portal.azure.com/) selezionare **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On](common/select-sso.png)

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare la modalità **SAML/WS-Fed** per abilitare il Single Sign-On.

    ![Selezione della modalità Single Sign-On](common/select-saml-option.png)

3. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona **Modifica** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Modificare la configurazione SAML di base](common/edit-urls.png)

4. Nella sezione **Configurazione SAML di base** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di Zoom](common/sp-identifier.png)

    a. Nella casella di testo **URL di accesso** digitare un URL nel formato seguente: `https://<companyname>.zoom.us`

    b. Nella casella di testo **Identificatore (ID entità)** digitare un URL usando il modello seguente: `<companyname>.zoom.us`

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'ID e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Zoom](https://support.zoom.us/hc/en-us). È anche possibile fare riferimento ai modelli mostrati nella sezione **Configurazione SAML di base** del portale di Azure.

5. L'applicazione Zoom prevede un formato specifico per le asserzioni SAML. È quindi necessario aggiungere mapping di attributi personalizzati alla configurazione degli attributi del token SAML. Lo screenshot seguente mostra l'elenco degli attributi predefiniti. Fare clic sull'icona  **Modifica** per aprire la finestra di dialogo **Attributi utente**. 

    ![image](common/edit-attribute.png)

6. Oltre quelli elencati in precedenza, l'applicazione Zoom prevede il passaggio di qualche altro attributo nella risposta SAML. Nella sezione **Attestazioni utente** della finestra di dialogo **Attributi utente** eseguire la procedura seguente per aggiungere l'attributo del token SAML come illustrato nella tabella seguente:
    
    | NOME | Spazio dei nomi  |  Attributo di origine|
    | ---------------| --------------- | --------- |
    | Indirizzo di posta elettronica  | user.mail  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail` |
    | Nome  | user.givenname  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |
    | Cognome  | user.surname  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |
    | Numero di telefono  | user.telephonenumber  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone` |
    | department  | user.department  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department` |
    | role |    user.assignedrole |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/role` |

    > [!NOTE]
    > Fare clic [qui](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) per sapere come configurare un Ruolo in Azure AD

    a. Fare clic su **Aggiungi nuova attestazione** per aprire la finestra di dialogo **Gestisci attestazioni utente**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Nella casella di testo **Nome** digitare il nome dell'attributo indicato per la riga.

    c. Per Origine selezionare **Attributo**.

    d. Nell'elenco **Attributo di origine** selezionare il valore dell'attributo indicato per la riga.

    e. Fare clic su **Ok**

    f. Fare clic su **Save**.

    > [!NOTE]
    > Zoom può prevedere l'attestazione dei gruppi nel payload SAML, di conseguenza se sono stati creati gruppi, contattare [il team di supporto clienti di Zoom](https://support.zoom.us/hc/en-us) e fornire le informazioni sui gruppi in modo che possano configurarle in Zoom. È necessario fornire al [team di supporto clienti di Zoom](https://support.zoom.us/hc/en-us) anche l'ID oggetto per consentirne la configurazione in Zoom. Per recuperare l'ID oggetto, consultare la [documentazione](https://support.zoom.us/hc/en-us/articles/115005887566).

7. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML**, fare clic su **Scarica** per scaricare il **Certificato (Base64)** definito dalle opzioni specificate in base ai propri requisiti e salvarlo in questo computer.

    ![Collegamento di download del certificato](common/certificatebase64.png)

8. Nella sezione **Configura Zoom** copiare gli URL appropriati in base alle proprie esigenze.

    ![Copiare gli URL di configurazione](common/copy-configuration-urls.png)

    a. URL di accesso

    b. Identificatore di Azure AD

    c. URL di chiusura sessione

### <a name="configure-zoom-single-sign-on"></a>Configurare l'accesso Single Sign-On per Zoom

1. In un'altra finestra del Web browser accedere al sito aziendale di Zoom come amministratore.

2. Fare clic sulla scheda **Single Sign-On** .

    ![Scheda Single Sign-On](./media/zoom-tutorial/ic784700.png "Single Sign-On")

3. Fare clic sulla scheda **Security Control** (Controllo di sicurezza) e quindi passare alle impostazioni **Single Sign-On**.

4. Nella sezione Single Sign-On seguire questa procedura:

    ![Sezione Single Sign-On](./media/zoom-tutorial/ic784701.png "Single Sign-On")

    a. Nella casella di testo **URL pagina di accesso** incollare il valore di **URL di accesso** copiato dal portale di Azure.

    b. Per il valore **URL pagina di disconnessione**, è necessario andare sul portale di Azure e fare clic su **Azure Active Directory** sulla sinistra, quindi passare a **Registrazioni app**.

    ![Pulsante Azure Active Directory](./media/zoom-tutorial/appreg.png)

    c. Fare clic su **Endpoint**

    ![Pulsante Endpoint](./media/zoom-tutorial/endpoint.png)

    d. Copiare **SAML-P SIGN-OUT ENDPOINT** e incollarlo nella casella di testo **URL pagina di disconnessione**.

    ![Pulsante Copia endpoint](./media/zoom-tutorial/endpoint1.png)

    e. Aprire il certificato con codifica base 64 nel blocco note, copiarne il contenuto negli appunti e quindi incollarlo nella casella di testo **Identity provider certificate** .

    f. Nella casella di testo **Issuer** (Autorità di certificazione) incollare il valore di **Identificatore Azure AD** copiato dal portale di Azure. 

    g. Fare clic su **Save**.

    > [!NOTE]
    > Per altre informazioni, visitare la documentazione relativa allo zoom [https://zoomus.zendesk.com/hc/articles/115005887566](https://zoomus.zendesk.com/hc/articles/115005887566)

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure, selezionare **Azure Active Directory**, **Utenti** e quindi **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](common/users.png)

2. Selezionare **Nuovo utente** in alto nella schermata.

    ![Pulsante Nuovo utente](common/new-user.png)

3. In Proprietà utente seguire questa procedura.

    ![Finestra di dialogo Utente](common/user-properties.png)

    a. Nel campo **Nome** immettere **BrittaSimon**.
  
    b. Nel campo **Nome utente** digitare **brittasimon\@dominioaziendale.estensione**  
    Ad esempio: BrittaSimon@contoso.com

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella Password.

    d. Fare clic su **Create**(Crea).

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione si abilita Britta Simon all'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Zoom.

1. Nel portale di Azure selezionare **Applicazioni aziendali**, quindi **Tutte le applicazioni** e infine **Zoom**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni digitare e selezionare **Zoom**.

    ![Collegamento di Zoom nell'elenco delle applicazioni](common/all-applications.png)

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"](common/users-groups-blade.png)

4. Fare clic sul pulsante **Aggiungi utente** e quindi selezionare **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione](common/add-assign-user.png)

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti e quindi fare clic sul pulsante **Seleziona** in basso nella schermata.

6. Se si prevede un valore di ruolo nell'asserzione SAML, nella finestra di dialogo **Selezionare un ruolo** selezionare il ruolo appropriato per l'utente dall'elenco, quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.

7. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.

### <a name="create-zoom-test-user"></a>Creare l'utente di test di Zoom

Per consentire agli utenti di Azure AD di accedere a Zoom, è necessario effettuarne il provisioning in Zoom. Nel caso di Zoom, il provisioning è un'attività manuale.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Per eseguire il provisioning di un account utente, seguire questa procedura:

1. Accedere al sito aziendale di **Zoom** come amministratore.

2. Fare clic sulla scheda **Gestione account** e quindi su **Gestione utente**.

3. Nella sezione Gestione utente fare clic su **Aggiungi utenti**.

    ![Gestione degli utenti](./media/zoom-tutorial/ic784703.png "Gestione degli utenti")

4. Nella pagina **Aggiungi utenti** seguire la procedura seguente:

    ![Aggiungere utenti](./media/zoom-tutorial/ic784704.png "Aggiungere utenti")

    a. In **Tipo utente** selezionare **Basic**.

    b. Nella casella di testo **Emails** (Messaggi di posta elettronica) digitare l'indirizzo e-mail di un account Azure AD di cui si vuole effettuare il provisioning.

    c. Fare clic su **Aggiungi**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente di Zoom per effettuare il provisioning degli account utente di Azure Active Directory.

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro di Zoom nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Zoom per cui si è configurato l'accesso SSO. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Che cos'è l'accesso condizionale in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
