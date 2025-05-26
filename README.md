        for _, player in players:GetChildren() do
            if (player == players.LocalPlayer) then
                continue;
            end
            
            local character = player.Character;

            if (not character) then
                continue;
            end

            local root = character.FindFirstChild(character, "HumanoidRootPart");

            if (not root) then
                continue;
            end

            local w2s, on_screen = camera.WorldToViewportPoint(camera, root.Position);

            if (not on_screen) then
                continue;
            end

            local center = (camera.ViewportSize / 2);
            local distance = (Vector2.new(w2s.X, w2s.Y) - center).Magnitude;

            if (range >= distance) then
                closest = character;
                range = distance;
            end
        end

        return closest;
    end
end

local old_bullet_handler = nil; old_bullet_handler = hookfunction(bullet_handler, function(origin_position, look_vector, oh_unknown1, oh_unknown2, ignore_list, ...)
    local closest = utility.get_closest_player(1000);
    table.insert(ignore_list, workspace.Map); -- this is the wallbang, remove this line or comment it out if you do not want it .

    if (closest) then
        local head = closest.FindFirstChild(closest, "Head");

        if (head) then
            print(origin_position);
            look_vector = (head.Position - origin_position).Unit * 10000; -- 😎😎😎😎🙏🙏🙏 funny insta hit
        end
    end

    return old_bullet_handler(origin_position, look_vector, oh_unknown1, oh_unknown2, ignore_list, ...);
end)
