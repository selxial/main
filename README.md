import discord
from discord.ext import commands
import random, os

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)

@bot.event
async def on_ready():
    print(f'it should look like this example {bot.user}')

@bot.command()
async def discuss(ctx):
    topics = [
        ("Kurangi penggunaan plastik dengan beralih ke tas yang dapat digunakan kembali.", "Anda dapat menyelesaikan ini dengan menggunakan tas kain."),
        ("Komposkan sisa makanan Anda untuk mengurangi sampah.", "Anda dapat menyelesaikan ini dengan memulai tempat kompos."),
        ("Daur ulang kertas dan karton untuk menyelamatkan pohon.", "Anda dapat menyelesaikan ini dengan memisahkan barang-barang daur ulang dari sampah."),
        ("Gunakan botol air yang dapat digunakan kembali daripada botol plastik sekali pakai.", "Anda dapat menyelesaikan ini dengan membawa botol yang dapat digunakan kembali."),
        ("Sumbangkan pakaian lama daripada membuangnya.", "Anda dapat menyelesaikan ini dengan mencari pusat donasi lokal."),
        ("Beralih ke penagihan digital untuk mengurangi limbah kertas.", "Anda dapat menyelesaikan ini dengan memilih tagihan elektronik."),
    ]

    topic, solution = random.choice(topics)
    text_message = f"Topik: {topic}\n\nUntuk menyelesaikan ini, gunakan perintah: $solve"

    await ctx.send(content=text_message)

@bot.command()
async def solve(ctx):
    topics = {
        "Kurangi penggunaan plastik dengan beralih ke tas yang dapat digunakan kembali.": "Anda dapat menyelesaikan ini dengan menggunakan tas kain.",
        "Komposkan sisa makanan Anda untuk mengurangi sampah.": "Anda dapat menyelesaikan ini dengan memulai tempat kompos.",
        "Daur ulang kertas dan karton untuk menyelamatkan pohon.": "Anda dapat menyelesaikan ini dengan memisahkan barang-barang daur ulang dari sampah.",
        "Gunakan botol air yang dapat digunakan kembali daripada botol plastik sekali pakai.": "Anda dapat menyelesaikan ini dengan membawa botol yang dapat digunakan kembali.",
        "Sumbangkan pakaian lama daripada membuangnya.": "Anda dapat menyelesaikan ini dengan mencari pusat donasi lokal.",
        "Beralih ke penagihan digital untuk mengurangi limbah kertas.": "Anda dapat menyelesaikan ini dengan memilih tagihan elektronik.",
    }

    
    async for message in ctx.channel.history(limit=100):
        if message.author == bot.user and message.content.startswith("Topik:"):
            last_topic = message.content.split("\n")[0].replace("Topik: ", "")
            solution_message = topics.get(last_topic, "Solusi tidak ditemukan.")
            break
    else:
        solution_message = "Tidak ada topik yang ditemukan. Silakan mulai diskusi terlebih dahulu menggunakan $discuss."

    await ctx.send(content=solution_message)

bot.run("token")
