<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login com Google</title>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f5f5f5;
        }
        
        #g_id_onload {
            position: absolute;
            left: -9999px;
        }
        
        .google-btn {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            background-color: #fff;
            color: #757575;
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 10px 16px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        
        .google-btn:hover {
            background-color: #f7f7f7;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <div id="g_id_onload"
         data-client_id="81771833811-vtorqavhk1c719eegn2mupm8s04c51q7.apps.googleusercontent.com"
         data-callback="handleCredentialResponse">
    </div>
    
    <div class="g_id_signin"
         data-type="standard"
         data-theme="outline"
         data-text="sign_in_with"
         data-shape="rectangular"
         data-logo_alignment="left">
    </div>

    <script>
        function handleCredentialResponse(response) {
            // Decodifica o token JWT
            const responsePayload = parseJwt(response.credential);
            
            // Salva os dados no localStorage
            localStorage.setItem('googleUser', JSON.stringify({
                email: responsePayload.email,
                picture: responsePayload.picture,
                name: responsePayload.name,
                token: response.credential
            }));
            
            // Redireciona para a dashboard
            window.location.href = 'dashboard.html';
        }
        
        function parseJwt(token) {
            const base64Url = token.split('.')[1];
            const base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            const jsonPayload = decodeURIComponent(atob(base64).split('').map(function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
            
            return JSON.parse(jsonPayload);
        }
    </script>
    
    <script src="https://accounts.google.com/gsi/client" async defer></script>
</body>
</html>
