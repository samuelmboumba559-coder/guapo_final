from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.lang import Builder
from kivy.clock import Clock
import urllib.request, json
from datetime import datetime

KV = """
<GuapoLayout>:
    orientation: 'vertical'
    padding: 15
    spacing: 10
    Label:
        text: 'GUAPO P2P GABON'
        font_size: '26sp'
        bold: True
        size_hint_y: 0.15
    BoxLayout:
        orientation: 'vertical'
        spacing: 12
        Label:
            id: heure
            text: 'Heure: --:--'
        Label:
            id: vendeur
            text: 'Vendeur: ---'
            font_size: '20sp'
        Label:
            id: acheteur
            text: 'Acheteur: ---'
            font_size: '20sp'
        Label:
            id: profit
            text: 'Profit 50k: 0 XAF'
            bold: True
    Button:
        id: btn
        text: 'DEMARRER'
        font_size: '20sp'
        background_color: 0,0.8,0,1
        size_hint_y: 0.3
        on_press: root.toggle()
"""

def get_prix(trade_type):
    url = "https://p2p.binance.com/bapi/c2c/v2/friendly/c2c/adv/search"
    data = json.dumps({"asset": "USDT", "fiat": "XAF", "page": 1, "rows": 1, "tradeType": trade_type, "payTypes": []}).encode()
    try:
        req = urllib.request.Request(url, data=data, headers={'Content-Type': 'application/json'})
        with urllib.request.urlopen(req, timeout=10) as r:
            return float(json.loads(r.read().decode())['data'][0]['adv']['price'])
    except:
        return 0

class GuapoLayout(BoxLayout):
    running = False
    def toggle(self):
        if not self.running:
            self.running = True
            self.ids.btn.text = "ARRETER"
            Clock.schedule_interval(self.update, 30)
            self.update(0)
        else:
            self.running = False
            self.ids.btn.text = "DEMARRER"
            Clock.unschedule(self.update)
    def update(self, dt):
        v = get_prix("BUY")
        a = get_prix("SELL")
        self.ids.heure.text = f"Heure: {datetime.now().strftime('%H:%M:%S')}"
        if v != 0:
            self.ids.vendeur.text = f"Vendeur: {v:.1f} XAF"
            self.ids.acheteur.text = f"Acheteur: {a:.1f} XAF"
            self.ids.profit.text = f"Profit: {((50000 / v) * (a - v)):.0f} XAF"

class GuapoApp(App):
    def build(self):
        Builder.load_string(KV)
        return GuapoLayout()

GuapoApp().run()
