cmake_minimum_required(VERSION 3.12)
project(ThreadPool)

add_library(ThreadPool OBJECT ThreadPool.cpp)
target_compile_features(ThreadPool PRIVATE cxx_std_17)
target_include_directories(ThreadPool PUBLIC .)
