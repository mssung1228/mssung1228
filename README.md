#include <stdio.h>
#include <SDL.h>
#include <SDL_image.h>

int main(int argc, char* argv[]) {
    // SDL 초기화
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        printf("SDL_Init Error: %s\n", SDL_GetError());
        return 1;
    }

    // SDL2 이미지 라이브러리 초기화
    if (IMG_Init(IMG_INIT_JPG | IMG_INIT_PNG) == 0) {
        printf("IMG_Init Error: %s\n", IMG_GetError());
        SDL_Quit();
        return 1;
    }

    // 윈도우와 렌더러 생성  
    SDL_Window* window = SDL_CreateWindow("JPG and PNG Display", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, 800, 600, SDL_WINDOW_SHOWN);
    if (!window) {
        printf("SDL_CreateWindow Error: %s\n", SDL_GetError());
        IMG_Quit();
        SDL_Quit();
        return 1;
    }

    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
    if (!renderer) {
        printf("SDL_CreateRenderer Error: %s\n", SDL_GetError());
        SDL_DestroyWindow(window);
        IMG_Quit();
        SDL_Quit();
        return 1;
    }

    // JPG 이미지 로드
    SDL_Texture* jpgTexture = IMG_LoadTexture(renderer, "jcshim.jpg");
    if (!jpgTexture) {
        printf("IMG_LoadTexture (jpg) Error: %s\n", IMG_GetError());
        SDL_DestroyRenderer(renderer);
        SDL_DestroyWindow(window);
        IMG_Quit();
        SDL_Quit();
        return 1;
    }

    // PNG 이미지 로드
    SDL_Texture* pngTexture = IMG_LoadTexture(renderer, "jcshim.png");
    if (!pngTexture) {
        printf("IMG_LoadTexture (png) Error: %s\n", IMG_GetError());
        SDL_DestroyTexture(jpgTexture);
        SDL_DestroyRenderer(renderer);
        SDL_DestroyWindow(window);
        IMG_Quit();
        SDL_Quit();
        return 1;
    }

    // 화면에 이미지 출력
    SDL_RenderClear(renderer);

    // JPG 이미지 출력
    SDL_Rect jpgRect = { 50, 50, 300, 200 };  // 위치 및 크기
    SDL_RenderCopy(renderer, jpgTexture, NULL, &jpgRect);

    // PNG 이미지 출력
    SDL_Rect pngRect = { 400, 50, 300, 200 };  // 위치 및 크기
    SDL_RenderCopy(renderer, pngTexture, NULL, &pngRect);

    SDL_RenderPresent(renderer);

    // 5초 동안 대기
    SDL_Delay(5000);

    // 리소스 해제
    SDL_DestroyTexture(jpgTexture);
    SDL_DestroyTexture(pngTexture);
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);

    // 종료
    IMG_Quit();
    SDL_Quit();

    return 0;
}
