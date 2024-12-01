import requests
import re

def check_wordpress_version(url):
    try:
        response = requests.get(url, timeout=5)
        if response.status_code == 200:
            match = re.search(r'content="(.*?)" name="generator"', response.text)
            if match:
                print(f"Versión de WordPress detectada: {match.group(1)}")
            else:
                print("No se pudo detectar la versión de WordPress.")
        else:
            print(f"Error al acceder a {url}, estado: {response.status_code}")
    except requests.RequestException as e:
        print(f"Error al intentar acceder a {url}: {e}")

def check_common_routes(url):
    endpoints = [
        "wp-admin",
        "wp-login.php",
        "readme.html",
        "xmlrpc.php",
        "wp-config.php.bak"
    ]
    print(f"Verificando rutas comunes en {url}...\n")
    for endpoint in endpoints:
        full_url = f"{url}/{endpoint}"
        try:
            response = requests.get(full_url, timeout=5)
            if response.status_code == 200:
                print(f"Riesgo potencial detectado: {full_url}")
            else:
                print(f"{full_url} no está accesible.")
        except requests.RequestException:
            print(f"No se pudo verificar {full_url}.")

def check_wp_config(url):
    wp_config_url = f"{url}/wp-config.php"
    try:
        response = requests.get(wp_config_url, timeout=5)
        if response.status_code == 200:
            print("El archivo wp-config.php está accesible públicamente. ¡ALERTA!")
        else:
            print("wp-config.php no es accesible.")
    except requests.RequestException:
        print("No se pudo verificar wp-config.php.")

def check_plugins_themes(url):
    plugins_to_check = [
        "akismet", "jetpack", "contact-form-7", "woocommerce", "wp-super-cache"
    ]
    for plugin in plugins_to_check:
        plugin_url = f"{url}/wp-content/plugins/{plugin}/"
        try:
            response = requests.get(plugin_url, timeout=5)
            if response.status_code == 200:
                print(f"Plugin vulnerable encontrado: {plugin} en {plugin_url}")
            else:
                print(f"{plugin} no está accesible.")
        except requests.RequestException:
            print(f"No se pudo verificar el plugin {plugin}.")
    
    themes_to_check = [
        "twentytwenty", "twentynineteen", "twentyeighteen"
    ]
    for theme in themes_to_check:
        theme_url = f"{url}/wp-content/themes/{theme}/"
        try:
            response = requests.get(theme_url, timeout=5)
            if response.status_code == 200:
                print(f"Tema vulnerable encontrado: {theme} en {theme_url}")
            else:
                print(f"{theme} no está accesible.")
        except requests.RequestException:
            print(f"No se pudo verificar el tema {theme}.")

def check_security_headers(url):
    try:
        response = requests.get(url, timeout=5)
        headers = response.headers
        print("\nComprobando encabezados de seguridad HTTP...")
        if "Strict-Transport-Security" not in headers:
            print("Falta Strict-Transport-Security (debe usar HTTPS).")
        if "X-Frame-Options" not in headers:
            print("Falta X-Frame-Options (potencial vulnerabilidad de clickjacking).")
        if "X-Content-Type-Options" not in headers:
            print("Falta X-Content-Type-Options (previene ataques MIME).")
        if "Content-Security-Policy" not in headers:
            print("Falta Content-Security-Policy (previene ataques XSS).")
    except requests.RequestException:
        print("No se pudo verificar los encabezados de seguridad.")

def scan_wordpress(url):
    print(f"Iniciando escaneo de WordPress en {url}...\n")
    check_wordpress_version(url)
    check_common_routes(url)
    check_wp_config(url)
    check_plugins_themes(url)
    check_security_headers(url)

url = input("Introduce la URL del sitio de WordPress a escanear: ")
scan_wordpress(url)
