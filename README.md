# ESP
EPS PARA ARSENAL ROBLOX(script incompleto para configuração propria
script incompleto para término de programação (partes separadas em etapas).!

-- Configurações
local esp_amigo_cor = Color3.fromRGB(0, 0, 255) -- Cor do ESP para amigos (azul)
local esp_inimigo_cor = Color3.fromRGB(255, 0, 0) -- Cor do ESP para inimigos (vermelho)
local esp_distancia = 100 -- Distância máxima para exibir o ESP
local anti_ban_delay = 0.1 -- Atraso anti-ban (em segundos)

-- Função para verificar a equipe do jogador
local function verificar_equipe(jogador)
  if jogador.TeamColor == game.Players.LocalPlayer.TeamColor then
    return esp_amigo_cor
  else
    return esp_inimigo_cor
  end
end

-- Função para desenhar o ESP
local function desenhar_esp(jogador)
  local posicao = jogador.Character.HumanoidRootPart.Position
  local distancia = (posicao - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude

  if distancia <= esp_distancia then
    local tela_posicao, visivel = workspace.CurrentCamera:WorldToScreenPoint(posicao)

    if visivel then
      local tamanho = Vector2.new(50, 20)
      local posicao_tela = Vector2.new(tela_posicao.X - tamanho.X / 2, tela_posicao.Y - tamanho.Y / 2)
      local esp_cor = verificar_equipe(jogador)

      -- Desenhar retângulo ao redor do jogador
      draw_rectangle(posicao_tela, tamanho, esp_cor)

      -- Exibir nome do jogador
      draw_text(jogador.Name, posicao_tela + Vector2.new(0, tamanho.Y + 5), esp_cor)
    end
  end
end

-- Loop principal
while true do
  wait(anti_ban_delay)

  for _, jogador in pairs(game.Players:GetPlayers()) do
    if jogador ~= game.Players.LocalPlayer and jogador.Character and jogador.Character:FindFirstChild("HumanoidRootPart") then
      desenhar_esp(jogador)
    end
  end
end
