-- ==========================================================
--                  TABELA DE CONFIGURAÇÕES
-- ==========================================================
local Settings = {
    JoinTeam = "Pirates" -- Coloque "Pirates" ou "Marines"
}

-- Puxa o ReplicatedStorage (usado nas suas funções de time)
local replicated = game:GetService("ReplicatedStorage")

-- Tabela que guarda as funções de trocar de time
local TeamFunctions = {
    Marines = function()
        replicated.Remotes.CommF_:InvokeServer("SetTeam", "Marines")
    end,
    
    Pirates = function()
        replicated.Remotes.CommF_:InvokeServer("SetTeam", "Pirates")
    end
}

-- ==========================================================
--          SISTEMA AUTOMÁTICO DE SELEÇÃO DE TIME
-- ==========================================================
-- Procura na tabela TeamFunctions a função com o nome que está em Settings.JoinTeam
local Escolha = TeamFunctions[Settings.JoinTeam]

if Escolha then
    -- Roda a função em uma thread separada para não travar o carregamento da UI
    task.spawn(function()
        -- Um pequeno delay de segurança para o jogo carregar antes de mandar o comando
        if not game:IsLoaded() then game.Loaded:Wait() end
        task.wait(0.5) 
        
        pcall(Escolha)
    end)
else
    warn("[Coelho Hub] Time inválido em Settings.JoinTeam! Use 'Pirates' ou 'Marines'.")
end
