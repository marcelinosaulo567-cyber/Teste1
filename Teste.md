--// 🧠 Carregar uma UI Library RedzLib
redzlib local = loadstring(jogo:HttpGet("https://raw.githubusercontent.com/tbao143/Library-ui/refs/heads/main/Redzhubui"))()

--// 🪟 Criar Janela Principal
Janela local = redzlib:MakeWindow({
    Título = "🔥 HUB MAX 🔥",
    Subtítulo = "por seu IB",
    SalvarPasta = "HubMaxData"
})

Janela:AddMinimizeButton({
    Botão = { Imagem = "rbxassetid://18751483361", BackgroundTransparency = 0 },
    Canto = { CornerRadius = UDim.new(1, 1) }
})

--// 🧩 Serviços e Variáveis
Jogadores locais = jogo:GetService("Jogadores")
local RunService = jogo:GetService("RunService")
TweenService local = jogo:GetService("TweenService")
Serviço de Entrada de Usuário local = jogo:GetService("Serviço de Entrada de Usuário")

jogador local = Jogadores.JogadorLocal
caractere local = player.Character ou player.CharacterAdded:Wait()
humanoide local = personagem:WaitForChild("Humanoide")
rootPart local = caractere:WaitForChild("HumanoidRootPart")

spawnPos local = nulo
voo local = falso
noclip local = falso
teletransporte localButton = nulo
destaques locaisHabilitado = falso

--// ⚡ Texto principal de Avisos
screenGui local = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "HubMaxGui"
screenGui.ResetOnSpawn = falso

topText local = Instância.new("TextLabel", screenGui)
topText.Size = UDim2.new(1, 0, 0, 30)
topText.BackgroundTransparency = 0,3
topText.BackgroundColor3 = Cor3.fromRGB(255, 255, 255)
topText.TextColor3 = Cor3.fromRGB(0, 0, 0)
topText.Font = Enum.Font.GothamBold
topText.TextSize = 20
topText.Visible = falso

traço local = Instance.new("UIStroke", topText)
traço.Cor = Cor3.deRGB(255, 0, 0)
traço.Espessura = 2

função local showTopText(msg, duração)
	topText.Visible = verdadeiro
	topText.Text = "⚡ " .. mensagem .. " ⚡"
	TweenService:Criar(topText, TweenInfo.new(0.4), {Transparência de Texto = 0}):Reproduzir()
	task.wait(duração ou 2)
	TweenService:Criar(topText, TweenInfo.new(1), {Transparência de Texto = 1}):Reproduzir()
	tarefa.espera(1)
	topText.Visible = falso
fim

--// 🚀 Aba Principal
local MainTab = Janela:MakeTab({ Nome = "Principal", Ícone = "rbxassetid://7734053429" })

Guia Principal:AdicionarBotão({
	Nome = "📍 Salvar Spawn",
	Retorno de chamada = função()
		se rootPart então
			spawnPos = rootPart.Posição
			showTopText("Salva de geração!", 1.5)
		fim
	fim
})

Guia Principal:AdicionarBotão({
	Name = "⚡ Teleportar (cria botão flutuante)",
	Retorno de chamada = função()
		se teleportButton então
			showTopText("Botão flutuante já existe!", 1.5)
			retornar
		fim

		-- Criar botão flutuante
		teleportButton = Instância.new("TextButton")
		teleportButton.Size = UDim2.new(0, 150, 0, 40)
		teleportButton.Position = UDim2.new(0,5, -75, 0,8, 0)
		teleportButton.Text = "⚡ Teletransportar"
		teleportButton.BackgroundColor3 = Cor3.fromRGB(255, 255, 255)
		teleportButton.TextColor3 = Color3.fromRGB(0, 0, 0)
		teleportButton.Font = Enum.Font.GothamBold
		teleportButton.TextSize = 18
		teleportButton.Active = verdadeiro
		teleportButton.Draggable = verdadeiro
		teleportButton.Parent = screenGui

		canto local = Instance.new("UICorner", teleportButton)
		canto.CornerRadius = UDim.new(0, 12)

		neon local = Instance.new("UIStroke", teleportButton)
		neon.Cor = Cor3.deRGB(255, 0, 0)
		neon.Espessura = 2

		teleportButton.MouseButton1Click:Conectar(função()
			se spawnPos então
				rootPart.CFrame = CFrame.new(spawnPos)
				humanoid.Sit = verdadeiro
				showTopText("Teleportado ao Spawn!", 1.2)
			outro
				showTopText("Salvo Nenhum Spawn!", 1.2)
			fim
		fim)

		showTopText("Botão flutuante criado!", 1.5)
	fim
})

Guia Principal:AdicionarBotão({
	Name = "🕊️ Flutuar até Spawn",
	Retorno de chamada = função()
		se não spawnPos então
			showTopText("Salve um Spawn primeiro!", 1.5)
			retornar
		fim

		showTopText("Flutuando até Spawn...", 1.5)
		bodyVelocity local = Instance.new("BodyVelocity", rootPart)
		bodyVelocity.MaxForce = Vetor3.novo(9e9, 9e9, 9e9)
		bodyVelocity.Velocidade = Vetor3.zero

		RunService.Heartbeat:Connect(função()
			se bodyVelocity e rootPart e spawnPos então
				direção local = (spawnPos - rootPart.Position).Unit
				bodyVelocity.Velocity = direção * 80
				se (rootPart.Position - spawnPos).Magnitude < 5 então
					bodyVelocity:Destruir()
					showTopText("Chegou ao Spawn!", 1.2)
				fim
			fim
		fim)
	fim
})

--// 🧱 Aba Extras
ExtraTab local = Janela:CriarTab({ Nome = "Extras", Ícone = "rbxassetid://7733918854" })

Guia Extra:AdicionarBotão({
	Nome = "🚪 Ativar Noclip",
	Retorno de chamada = função()
		noclip = não noclip
		showTopText(noclip e "Noclip Ativado" ou "Noclip Desativado", 1.5)
	fim
})

RunService.Stepped:Connect(função()
	se noclip e personagem então
		para _, parte em pares(character:GetDescendants()) faça
			se parte:IsA("BasePart") então
				part.CanCollide = falso
			fim
		fim
	fim
fim)

Guia Extra:AdicionarBotão({
	Name = "💫 Ativar Destaques",
	Retorno de chamada = função()
		highlightsEnabled = não highlightsEnabled
		para _, v em pares(workspace:GetDescendants()) faça
			se v:IsA("Modelo") e v:FindFirstChild("Humanoid") e v:FindFirstChild("HumanoidRootPart") então
				local existente = v:FindFirstChildOfClass("Destaque")
				se highlightsEnabled e não existir então
					local h = Instance.new("Destaque", v)
					h.FillColor = Cor3.fromRGB(255, 255, 255)
					h.OutlineColor = Cor3.fromRGB(255, 0, 0)
				elseif não highlightsEnabled e existente então
					existente:Destruir()
				fim
			fim
		fim
		showTopText(highlightsEnabled and "Destaques Ativados" ou "Destaques Desativados", 1.5)
	fim
})

--// 🔥 Mensagem final
showTopText("🔥 HUB MAX carregado com sucesso! 🔥", 3)
print("✅ HUB MAX iniciado com sucesso.")
