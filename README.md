_G.FarmAll = true
 function click()
        local vi = game:service'VirtualInputManager'
        vi:SendMouseButtonEvent(500,500, 0, true, game, 1)
        wait()
        vi:SendMouseButtonEvent(500,500, 0, false, game, 1)
    end

    function Mod()
        for i,v in next, game.ReplicatedStorage.Weapons:GetChildren() do
            for i,c in next, v:GetChildren() do
                for i,x in next, getconnections(c.Changed) do
                    x:Disable()
                    end
                    if c.Name == "Ammo" or c.Name == "StoredAmmo" then
                    c.Value = 99
                    elseif c.Name == "AReload" or c.Name == "RecoilControl" or c.Name == "EReload" or c.Name == "SReload" or c.Name == "ReloadTime" or c.Name == "EquipTime" or c.Name == "Spread" or c.Name == "MaxSpread" then
                    c.Value = 0
                    elseif c.Name == "Range" then
                    c.Value = 9e9
                    elseif c.Name == "Auto" then
                    c.Value = true
                    elseif c.Name == "FireRate" or c.Name == "BFireRate" then
                    c.Value = 0.02
                end
            end
        end
    end
    
    Mod()

    task.spawn(function()
        while task.wait() do
            if _G.FarmAll then
                for i ,v in pairs(game.Players:GetChildren()) do
                    if v ~= game.Players.LocalPlayer and v.TeamColor ~= game.Players.LocalPlayer.TeamColor and v.Character and v.Character:FindFirstChild("Gun") then
                        repeat wait()
                            pcall(function()
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,49,false,game.Players.LocalPlayer.Character.HumanoidRootPart)
                                wait()
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,49,false,game.Players.LocalPlayer.Character.HumanoidRootPart)
                                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame + (v.Character.Head.CFrame.LookVector * -3)
                                game.Workspace.CurrentCamera.CoordinateFrame  = CFrame.new(game.Workspace.CurrentCamera.CoordinateFrame.p , v.Character.Head.CFrame.p)
                                task.spawn(click)
                            end)
                        until _G.FarmAll == false or not v.Character.Parent or v.Character.Humanoid:FindFirstChild('Died')
                    end    
                end
            end
        end
    end)
