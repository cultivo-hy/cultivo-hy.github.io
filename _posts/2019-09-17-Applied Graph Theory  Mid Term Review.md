---
layout: post

title: "[Graph] 응용 그래프(Applied Graph Theory)"

tags:

- graph
- algorithm
- NP-complete
- Tool
- programming

category:
- graph
- algoithm
- NP-complete
- Delaunay

---
* toc
{:toc}


# 응용그래프 (Applied Graph Theory) 중간고사 최종 보고서

##### 코드

```
import sys
import matplotlib.pyplot as plt
import numpy as np
import networkx as nx
import math
import concavehull as cv
from scipy.spatial import Delaunay
from scipy.spatial import ConvexHull
from itertools import product

numbed_of_point = 0         # The number of point
threshold = 0         # Threshold value ( vertex to cycle )
point_list = []          # Point [ [x1, y1], [x2, y2], ...]  list
cycle_vertex_list = []          # Cycle vertex [ v1, v2, .. ] list
cycle_length = 1000 * 1000    # The Length of Cycle
center= 0                     # center of the cycle
final_center = 0

city_f = open('city-100.txt','r')
cycle_f = open('citycycle-100.txt','w')

def getDistance(first_vertex, second_vertex):
    tmp1, tmp2 = point_list[first_vertex], point_list[second_vertex]
    x_diff , y_diff = tmp2[0] - tmp1[0] , tmp2[1] - tmp1[1]
    return round(math.sqrt(x_diff**2+y_diff**2),3)

def get_vertex_num(tpoint):
    for iter_i in range(len(point_list)):
        if point_list[iter_i] == tpoint:
            return iter_i

def get_cycle_length(cycle_list_parameter):
    cycle_lens = 0
    for cycle_list_iter in range(len(cycle_list_parameter) - 1):
        tmp_start, tmp_end = cycle_list_parameter[cycle_list_iter], cycle_list_parameter[cycle_list_iter + 1]
        cycle_lens += edgeLengthList[tmp_start - 1][tmp_end - 1]
    cycle_lens = round(cycle_lens, 3)
    return cycle_lens

def get_inserted_concave(target_value, concave_vertex_parameter, cycle_make_from_to_parameter):
    local_temp_cycle_candidate = []
    for count_index in range(len(concave_vertex_parameter)):
        temp_concave_vertex_list = concave_vertex_parameter.copy()
        temp_concave_vertex_list.insert(count_index,target_value)
        temporary_cycle, temporary_vertex = create_cycle_from_concave(temp_concave_vertex_list)
        temporary_cycle = remove_duplicated_vertex(temporary_cycle, temporary_vertex)
        temporary_cycle = optimize_concaved_cycle(temporary_cycle, temp_concave_vertex_list, cycle_make_from_to_parameter)
        temporary_cycle.append(temporary_cycle[0])
        temp_cycle_length = get_cycle_length(temporary_cycle)
        if temporary_cycle[0] == temporary_cycle[-1] and len(temporary_cycle[1:]) == len(set(temporary_cycle[1:])):
            local_temp_cycle_candidate.append([temp_concave_vertex_list,temporary_cycle, temp_cycle_length])
    if len(local_temp_cycle_candidate) != 0:
        tmp_result_list = sorted(local_temp_cycle_candidate, key=lambda x: x[2])[0]
        return_concave = []
        for cycle_it in tmp_result_list[1]:
            if cycle_it in tmp_result_list[0]:
                return_concave.append(cycle_it)
        tmp_result_list[0] = return_concave
        return tmp_result_list
    return 0

def create_cycle_from_concave(concave_parameter):
    new_cycle_list = []
    new_vertex_check_list = [0 for i in range(numbed_of_point)]
    for concave_iter in range(len(concave_parameter)-1):
        concave_start, concave_end = concave_parameter[concave_iter], concave_parameter[concave_iter+1]
        shortest_path_list = nx.shortest_path(Graph, concave_start, concave_end, weight='weight')
        for shortest_path_iter in range(len(shortest_path_list)-1):
            new_cycle_list.append(shortest_path_list[shortest_path_iter])
            new_vertex_check_list[shortest_path_list[shortest_path_iter]-1] += 1

    concave_start = concave_parameter[-1]   # center가 마지막 concave에 포함되는 경우 추가해줘야 함
    concave_end = concave_parameter[0]
    if concave_start != concave_end:
        shortest_path_list = nx.shortest_path(Graph, concave_start, concave_end, weight='weight')
        for shortest_path_iter in range(len(shortest_path_list) - 1):
            new_cycle_list.append(shortest_path_list[shortest_path_iter])
            new_vertex_check_list[shortest_path_list[shortest_path_iter] - 1] += 1

    return new_cycle_list, new_vertex_check_list

def remove_duplicated_vertex(cycle_list_parameter, vertex_check_list_parameter):
    for vertex_check_iter in range(len(vertex_check_list_parameter)):
        compare_path = []
        if vertex_check_list_parameter[vertex_check_iter] == 2:
            for c_ind in range(len(cycle_list_parameter)):
                if vertex_check_iter + 1 == cycle_list_parameter[c_ind]:
                    cycle_start = cycle_list_parameter[c_ind - 1]
                    cycle_end = cycle_list_parameter[c_ind + 1] if c_ind != len(cycle_list_parameter) - 1 else cycle_list_parameter[0]
                    compare_path.append([cycle_start, cycle_end, c_ind])

            if (compare_path[0][1] == compare_path[1][0] and compare_path[0][1] == vertex_check_iter + 1) or \
                    (compare_path[0][0] == compare_path[1][1] and compare_path[0][0] == vertex_check_iter + 1):
                del cycle_list_parameter[compare_path[0][2]]
            else:
                test = Graph.copy()
                remove_vertex = vertex_check_iter+1
                test.remove_node(remove_vertex)
                if remove_vertex != compare_path[0][0] and remove_vertex != compare_path[0][1] and \
                        remove_vertex != compare_path[1][0] and remove_vertex != compare_path[1][1]:
                    first_flag = nx.has_path(test, compare_path[0][0], compare_path[0][1])
                    second_flag = nx.has_path(test, compare_path[1][0], compare_path[1][1])
                    if first_flag == False and second_flag == False:
                        del cycle_list_parameter[compare_path[0][2]]
                    else:
                        removed_index = -1
                        if first_flag == True and second_flag == True:
                            first_shortest = nx.shortest_path_length(test, compare_path[0][0], compare_path[0][1],weight='weight')
                            second_shortest = nx.shortest_path_length(test, compare_path[1][0], compare_path[1][1], weight='weight')
                            removed_index = 0 if first_shortest < second_shortest else 1
                        elif first_flag:
                            removed_index = 0
                        else:
                            removed_index = 1
                        remove_path = nx.shortest_path(test, compare_path[removed_index][0], compare_path[removed_index][1], weight='weight')
                        remove_list = []
                        for remove_path_iter in range(1, len(remove_path) - 1):
                            remove_list.append(remove_path[remove_path_iter])
                        insert_index = compare_path[removed_index][2]
                        del cycle_list_parameter[insert_index]
                        for remove_list_iter in remove_list:
                            cycle_list_parameter.insert(insert_index, remove_list_iter)
                            insert_index += 1

    return cycle_list_parameter

def optimize_concaved_cycle(cycle_list_parameter, concave_vertex_parameter, cycle_make_from_to_parameter): # 최적화 가능한 concave_vertex 제거
    remove_list = []
    for concave_iter in concave_vertex_parameter:
        checklist = []
        for cycle_make_from_iter in cycle_make_from_to_parameter:
            if cycle_make_from_iter[1] == concave_iter:
                checklist.append(cycle_make_from_iter)
        if concave_iter in cycle_list_parameter:
            concave_index = cycle_list_parameter.index(concave_iter)
            for cycle_make_iter in cycle_make_from_to_parameter:
                vertex_from, vertex_to = cycle_make_iter[0], cycle_make_iter[1]
                if vertex_to == concave_iter:
                    temp_cycle_list = list(set(cycle_list_parameter) - set([concave_iter]))
                    for cycle_it in temp_cycle_list:
                        left = len(cycle_list_parameter)-1 if concave_index == 0 else concave_index - 1
                        right = 0 if concave_index == len(cycle_list_parameter)-1 else concave_index + 1
                        compare = [vertex_from, concave_iter]
                        if threshold > edgeLengthList[vertex_from - 1][cycle_it - 1] and Graph.has_edge(cycle_list_parameter[left], cycle_list_parameter[right]):
                            if compare in checklist:
                                checklist.remove([vertex_from, concave_iter])
                                if len(checklist) == 0:
                                    remove_list.append(concave_iter)
                                    break
    remove_list = list(set(remove_list))
    for remove_list_iter in remove_list:
        cycle_list_parameter.remove(remove_list_iter)
        concave_vertex_parameter.remove(remove_list_iter)
    return cycle_list_parameter

def is_all_vertex_in_threshold(cycle_parameter):
    threshold_result = True
    for i in range(1, len(point_list) + 1):
        if i in cycle_parameter: continue
        else:
            minimum_dist = 1000 * 1000
            for j in cycle_parameter:
                current_dist = nx.shortest_path_length(Graph,i,j,weight='weight')
                if current_dist < minimum_dist:
                    minimum_dist = current_dist
            if minimum_dist > threshold:
                threshold_result = False
                break
    return threshold_result

def get_optimal_cycle(cycle_parameter):
    tmp_cycle_parameter = cycle_parameter.copy()
    for cycle_rm_it in range(1, len(tmp_cycle_parameter)):
        is_removed = False
        neighbor_flag = False
        if cycle_rm_it == len(tmp_cycle_parameter)-1 or cycle_rm_it == len(tmp_cycle_parameter):
            neighbor_flag = Graph.has_edge(tmp_cycle_parameter[cycle_rm_it -1], tmp_cycle_parameter[1])
            if neighbor_flag:
                del tmp_cycle_parameter[0]  # 처음 끝 제거
                del tmp_cycle_parameter[-1]  
                tmp_cycle_parameter.append(tmp_cycle_parameter[0]) # 마지막 꺼 추가
                is_removed = True
            break
        else:
            neighbor_flag = Graph.has_edge(tmp_cycle_parameter[cycle_rm_it + 1], tmp_cycle_parameter[cycle_rm_it - 1])
            if neighbor_flag:
                del tmp_cycle_parameter[cycle_rm_it]
                is_removed = True
        if is_removed:
            if is_all_vertex_in_threshold(tmp_cycle_parameter):
                cycle_parameter = tmp_cycle_parameter.copy()
            else:
                tmp_cycle_parameter = cycle_parameter.copy()
    return cycle_parameter



numbed_of_point, threshold = map(int, city_f.readline().split())
for line in city_f :
    p = list(map(int, line.split()))
    if p in point_list:
        print( "invalid point input")
        sys.exit()
    else : point_list.append(p)

# make base Delauny graph
tri = Delaunay(point_list)
PointArray = np.array(point_list)

Graph = nx.Graph()  # Graph
for i in range(1, numbed_of_point + 1) :
   Graph.add_node(i, pos=point_list[i - 1])

for t in tri.simplices.copy() :
    Graph.add_edge(t[0] + 1, t[1] + 1, color='black', thickness=1, weight=getDistance(t[0], t[1]))
    Graph.add_edge(t[1] + 1, t[2] + 1, color='black', thickness=1, weight=getDistance(t[1], t[2]))
    Graph.add_edge(t[0] + 1, t[2] + 1, color='black', thickness=1, weight=getDistance(t[0], t[2]))

# make all_path_length
path = dict(nx.all_pairs_dijkstra_path_length(Graph))
edgeLengthList = []
for node1 in Graph.node:
    tmp_edge_len = []
    for node2 in Graph.node:
        tmp_edge_len.append(path[node1][node2])
    edgeLengthList.append(tmp_edge_len)

hull = ConvexHull(point_list)
convex = hull.simplices
modiConvex = list(hull.vertices)
modiConvex = [ i+1 for i in modiConvex ]

#find graph center-closed vertex
X , Y = PointArray[:, 0],  PointArray[:, 1]
X_new , Y_new = sum(list(X))/len(X), sum(list(Y))/len(Y)
plt.plot(X_new,Y_new,'k+')

minimum =  math.sqrt((X_new - point_list[0][0]) ** 2 + (Y_new - point_list[0][1]) ** 2)
shortest_candidate_list = []
for a in range(len(point_list)):
    shortest_candidate_list.append([a, math.sqrt((X_new - point_list[a][0]) ** 2 + (Y_new - point_list[a][1]) ** 2)])
shortest_candidate_list = sorted(shortest_candidate_list, key=lambda x: x[1])[0:numbed_of_point]



for iteration in shortest_candidate_list:
    cycle_make_list = []            # before making cycle, check vertex
    cycle_make_from_to = []
    center = iteration[0]
    cycle_make_list.append(center+1)
    print('center is {}'.format(center+1))

    for modiInd in modiConvex:
        lengthSum = 0
        path = nx.shortest_path(Graph, modiInd, center + 1, weight='weight')
        checkBool, checkLength, tmpvertex = True, 0, 0
        for j in range(len(path) - 1):
            length = edgeLengthList[path[j] - 1][path[j + 1] - 1]
            if lengthSum + length <= threshold:
                lengthSum += length
            else:
                checkLength = edgeLengthList[path[j] - 1][path[-1] - 1]
                checkBool = False
                tmpvertex = path[j]
                break

        if checkBool is False:  # neighbor 중에서 W0가 더 작으면서 center와 거리가 더 가까워지는 애가 있으면 선택 (항상 최적은 아님)
            isEnough = True
            while isEnough is True:
                isEnough = False
                neighbor = [i for i in Graph.neighbors(tmpvertex)]
                for ind in neighbor:
                    tmpLength = edgeLengthList[ind - 1][tmpvertex - 1]
                    if tmpLength + lengthSum <= threshold and edgeLengthList[ind - 1][center] <= checkLength:
                        lengthSum = tmpLength + lengthSum
                        tmpvertex = ind
                        checkLength = edgeLengthList[ind - 1][center]
                        isEnough = True
                cycle_make_list.append(tmpvertex)
                cycle_make_from_to.append([modiInd, tmpvertex])
        else:
            cycle_make_from_to.append([modiInd, center + 1])

    #cycle vertex to concaveHull
    cycle_make_list = list(set(cycle_make_list))

    li = [ point_list[i-1] for i in cycle_make_list]
    li = np.array(li)
    concave = cv.concaveHull(li,3)
    concave_vertex = [get_vertex_num(list(i)) + 1 for i in concave]

    # make cycle using shortest path between concaveHaull
    cycle_list, vertex_check_list = create_cycle_from_concave(concave_vertex)
    cycle_list = remove_duplicated_vertex(cycle_list, vertex_check_list)
    cycle_list = optimize_concaved_cycle(cycle_list, concave_vertex, cycle_make_from_to)

    # vertex between cycle and convex & cycle inside
    add_list = []
    for it1 in range(1, len(point_list) + 1):
        if it1 in cycle_list or it1 in modiConvex:          continue
        else:
            cur_vertex , min_dist = 0, 1000 * 1000
            for it2 in cycle_list:
                cur_dist , it1_to_it2_path = 0, nx.shortest_path(Graph,it1,it2,weight='weight')
                for k in range(len(it1_to_it2_path) - 1):
                    cur_dist += edgeLengthList[it1_to_it2_path[k]-1][it1_to_it2_path[k+1]-1]
                if cur_dist < min_dist:
                    min_dist , cur_vertex = cur_dist , it2
            if min_dist > threshold:            # CYCLE 에 가장 가까운 게 threshold 보다 크면 추가해야 됨
                add_list.append(it1)
            else:
                cycle_make_from_to.append([it1,cur_vertex])


    if len(add_list)==0: # all conditions met
        cycle_list.append(cycle_list[0])
        cycle_list = get_optimal_cycle(cycle_list)
        if cycle_list[0] == cycle_list[-1] and len(cycle_list[1:]) == len(set(cycle_list[1:])):
            if is_all_vertex_in_threshold(cycle_list):
                lens = get_cycle_length(cycle_list)
                if lens == cycle_length:
                    print('find a same optimal cycle')
                elif lens < cycle_length:
                    cycle_vertex_list = cycle_list
                    cycle_length = lens
                    final_center = center + 1
                    print(lens)
                    print('new optimal length cycle is {}'.format(cycle_vertex_list))
                else:
                    print('not optimal cycle')
                continue
            else:
                print("threshold condition not met.")
        else:
            print("cycle condition not met")

    add_vertex_candidate_list = []  # 더 추가할 것이 있을 경우
    for it1 in add_list:
        checkLength, checkVertex, tmpVertex = 0, [], -1
        for it2 in cycle_list:
            lengthSum = 0
            path = nx.shortest_path(Graph, it1, it2, weight='weight')
            for j in range(len(path) - 1):
                length = edgeLengthList[path[j] - 1][path[j + 1] - 1]
                if lengthSum + length < threshold:
                    lengthSum += length
                else:
                    tmpVertex = path[j]
                    break
            if checkLength < lengthSum:
                checkLength = lengthSum
                checkVertex.append(tmpVertex)
        if len(checkVertex) != 0:
            add_vertex_candidate_list.append([it1, checkVertex])
        else:
            add_vertex_candidate_list.append([it1, [it1]])


    add_vertex_candidate = []   # 1개짜리 remove 한 뒤 포함되어 있는 다른 것 다 제거.
    remove_one_List = []
    for index in add_vertex_candidate_list:
        if len(index[1]) == 1:
            add_vertex_candidate.append(index[1][0])
            if index not in remove_one_List:
                remove_one_List.append(index)
            for value in add_vertex_candidate_list:
                if value[0] == index[1][0] and value not in remove_one_List:
                    remove_one_List.append(value)
                for val in value[1]:
                    if val == index[1][0] and value not in remove_one_List:
                        remove_one_List.append(value)

    for i in remove_one_List:
        add_vertex_candidate_list.remove(i)
    can_remove = True
    add_vertex_candidate = list(set(add_vertex_candidate))  # 제거한 것 concave 어디에 위치시킬 지 결정하기
    for add_vertex_iter in add_vertex_candidate:
        result_list = get_inserted_concave(add_vertex_iter, concave_vertex, cycle_make_from_to)
        if result_list == 0:    # 여기서 안 될 경우 뒤에서 되는것 고려
            continue
        concave_vertex = result_list[0]

    check_bool = True
    result_zero = False
    while check_bool:
        check_count = []
        count_List = []
        for index in add_vertex_candidate_list:
            for candidate_iter in index[1]:
                if candidate_iter not in count_List:
                    count_List.append(candidate_iter)
                    check_count.append([candidate_iter,[index[0]]])
                else:
                    for i in check_count:
                        if i[0] == candidate_iter:
                            i[1].append(index[0])
        comp , target_vertex  = 1 , 0

        for it in check_count:
            if comp < len(it[1]):
                target_vertex, comp = it[0], len(it[1])

        if target_vertex != 0:  #하나 이상의 vertex가 겹치면
            result_list = get_inserted_concave(target_vertex, concave_vertex, cycle_make_from_to)
            if result_list ==0:
                result_zero = True
                break
            concave_vertex = result_list[0]
            cycle_list = result_list[1]
            remove_one_List = []
            for value in add_vertex_candidate_list:      # 이때 추가한 target 을 포함하고 있는 add_vertex_list 제거해주기  위에 함
                for val in value[1]:
                    if val == target_vertex and value not in remove_one_List:
                        remove_one_List.append(value)
                if value[0] == target_vertex:
                    remove_one_List.append(value)
            for i in remove_one_List:
                add_vertex_candidate_list.remove(i)
        else:
            check_bool = False

    if result_zero:
        print("threshold condition not met.")
        continue

    for ind in add_vertex_candidate:
        for ind2 in add_vertex_candidate_list:
            if ind in ind2[1]:
                add_vertex_candidate.append(ind2[0])
                add_vertex_candidate_list.remove(ind2)

    #1개 짜리 확인용 cycle 생성
    cycle_list.clear()
    for it in range(len(concave_vertex) - 1):
        start, end = concave_vertex[it], concave_vertex[it + 1]
        path_list = nx.shortest_path(Graph, start, end, weight='weight')
        for it2 in range(len(path_list) - 1):
            cycle_list.append(path_list[it2])
    start, end = concave_vertex[-1] , concave_vertex[0]
    if start != end:
        path_list = nx.shortest_path(Graph, start, end, weight='weight')
        for iter2 in range(len(path_list) - 1):
            cycle_list.append(path_list[iter2])

    # 1개짜리 combination
    exist_flag = False
    append_list = []
    for it in add_vertex_candidate_list:
        exist_flag = False
        for it2 in it[1]:
            if it2 in cycle_list:
                exist_flag = True
                break
        if not exist_flag: append_list.append(it[1])

    if len(append_list) != 0:                           # 2개 이상일 때
        final_cycle_candidate = []
        if len(append_list) == 1:                             #1개 인 경우 combination 없이 두개 비교해서 답
            for add_iter in append_list[0]:
                temp_cycle_candidate = []
                result_list = get_inserted_concave(add_iter, concave_vertex, cycle_make_from_to)
                if result_list == 0:
                    continue
                final_cycle_candidate.append(result_list)
        else :
            append_list = list(product(*append_list))               # 여러 개인 경우 combination 들을 비교,
            append_list = [list(i) for i in append_list]
            final_cycle_candidate = []
            for list_val in append_list:
                val_1, val_2 = list_val[0] , list_val[1]
                tmp_final_cycle_candidate = []

                for concave_index_1 in range(len(concave_vertex)):
                    for concave_index_2 in range(len(concave_vertex)):
                        concave_tmp = concave_vertex.copy()
                        concave_tmp.insert(concave_index_1, val_1)
                        concave_tmp.insert(concave_index_2, val_2)
                        tem_cycle, tem_vertex = create_cycle_from_concave(concave_tmp)
                        tem_cycle = remove_duplicated_vertex(tem_cycle, tem_vertex)
                        tem_cycle = optimize_concaved_cycle(tem_cycle, tem_vertex, cycle_make_from_to)
                        tem_cycle.append(tem_cycle[0])
                        tem_cycle_length = get_cycle_length(tem_cycle)
                        if tem_cycle[0] == tem_cycle[-1] and len(tem_cycle[1:]) == len(set(tem_cycle[1:])):
                            tmp_final_cycle_candidate.append([concave_tmp, tem_cycle, tem_cycle_length])

                for concave_index_3 in range(len(concave_vertex)):
                    concave_tmp = concave_vertex.copy()
                    concave_tmp.insert(concave_index_3, val_2)
                    concave_tmp.insert(concave_index_3, val_1)
                    tem_cycle, tem_vertex = create_cycle_from_concave(concave_tmp)
                    tem_cycle = remove_duplicated_vertex(tem_cycle, tem_vertex)
                    tem_cycle = optimize_concaved_cycle(tem_cycle, tem_vertex, cycle_make_from_to)
                    tem_cycle.append(tem_cycle[0])
                    tem_cycle_length = get_cycle_length(tem_cycle)
                    if tem_cycle[0] == tem_cycle[-1] and len(tem_cycle[1:]) == len(set(tem_cycle[1:])):
                        tmp_final_cycle_candidate.append([concave_tmp, tem_cycle, tem_cycle_length])

                if len(tmp_final_cycle_candidate) != 0:
                    tmp_result_list = sorted(tmp_final_cycle_candidate, key=lambda x: x[2])[0]
                    final_cycle_candidate.append(tmp_result_list)

        if len(final_cycle_candidate) != 0:
            cycle_data = sorted(final_cycle_candidate, key=lambda x: x[2])[0]
            concave_vertex = cycle_data[0]
            cycle_list = cycle_data[1]

    final_list = []                     # 마지막으로 추가 안된 vertex 한번 더 추가
    for it1 in range(1, len(point_list) + 1):
        if it1 not in cycle_list:
            cur_vertex, min_dist = 0, 1000 * 1000
            for it2 in cycle_list:
                cur_dist, it1_to_it2_path = 0, nx.shortest_path(Graph, it1, it2, weight='weight')
                for k in range(len(it1_to_it2_path) - 1):
                    cur_dist += edgeLengthList[it1_to_it2_path[k] - 1][it1_to_it2_path[k + 1] - 1]
                if cur_dist < min_dist:
                    min_dist, cur_vertex = cur_dist, it2
            if min_dist > threshold:  # CYCLE 에 가장 가까운 게 threshold 보다 크면 추가해야 됨
                final_list.append(it1)

    result_zero = False
    if len(final_list) != 0:
        make_cycle_possible = True
        for final_index in final_list:
            if final_index in concave_vertex: continue
            can_go_cycle = False
            for cycle_ind in cycle_list:
                final_to_cycle_length = nx.shortest_path_length(Graph,final_index,cycle_ind,weight='weight')
                if final_to_cycle_length <= threshold:
                    can_go_cycle = True
                    break
            if can_go_cycle: continue

            cur_vertex, tmp_cycle_candidate = [] , []
            for cycle_index in cycle_list:
                cur_dist, final_to_cycle_path = 0, nx.shortest_path(Graph, final_index, cycle_index, weight='weight')
                for k in range(len(final_to_cycle_path) - 1):
                    length = edgeLengthList[final_to_cycle_path[k] - 1][final_to_cycle_path[k + 1] - 1]
                    if cur_dist + length < threshold:
                        cur_dist += length
                        if final_to_cycle_path[k+1] not in cur_vertex:
                            cur_vertex.append(final_to_cycle_path[k+1])
                    else:   break

            cur_vertex = list(set(cur_vertex))  # 갈수 있는 vertex 후보
            for vertex_value in cur_vertex:
                tmp_concave_vertex_list = concave_vertex.copy()
                result = get_inserted_concave(vertex_value, tmp_concave_vertex_list, cycle_make_from_to)
                if result == 0:
                    continue
                tmp_cycle_candidate.append(result)

            if len(tmp_cycle_candidate) != 0:
                tmp_cycle_data = sorted(tmp_cycle_candidate, key=lambda x: x[2])[0]
                concave_vertex = tmp_cycle_data[0]
                cycle_list = tmp_cycle_data[1]
            else:
                make_cycle_possible = False
                break
        if not make_cycle_possible:
            print("can't make cycle")
            continue

        cycle_list = get_optimal_cycle(cycle_list)
        if cycle_list[0] == cycle_list[-1] and len(cycle_list[1:]) == len(set(cycle_list[1:])):
            if is_all_vertex_in_threshold(cycle_list):
                lens = get_cycle_length(cycle_list)
                if lens == cycle_length:
                    print('find a same optimal cycle')
                elif lens < cycle_length:
                    cycle_vertex_list = cycle_list
                    cycle_length = lens
                    final_center = center + 1
                    print(lens)
                    print('new optimal length cycle is {}'.format(cycle_vertex_list))
                else:
                    print('not optimal cycle')
                continue
            else:
                print("threshold condition not met.")
        else:
            print("cycle condition not met")
    else:
        if len(append_list) == 0:            # append_list==0 이면 cycle 다시 making 해야함
            tmp_cycle, tmp_vertex = create_cycle_from_concave(concave_vertex)
            tmp_cycle = remove_duplicated_vertex(tmp_cycle, tmp_vertex)
            tmp_cycle = optimize_concaved_cycle(tmp_cycle, concave_vertex, cycle_make_from_to)
            tmp_cycle.append(tmp_cycle[0])
            cycle_list = tmp_cycle

        cycle_list = get_optimal_cycle(cycle_list)
        if cycle_list[0] == cycle_list[-1] and len(cycle_list[1:]) == len(set(cycle_list[1:])):
            if is_all_vertex_in_threshold(cycle_list):
                if lens == cycle_length:
                    print('find a same optimal cycle')
                elif lens < cycle_length:
                    cycle_vertex_list = cycle_list
                    cycle_length = lens
                    final_center = center + 1
                    print(lens)
                    print('new optimal length cycle is {}'.format(cycle_vertex_list))
                else:
                    print('not optimal cycle')
                continue
            else:
                print("threshold condition not met.")
        else:
            print("cycle condition not met")

for vertex_index in range(len(cycle_vertex_list)-1):
    start, end = cycle_vertex_list[vertex_index], cycle_vertex_list[vertex_index+1]
    edge_len = edgeLengthList[start - 1][end - 1]
    Graph.add_edge(start, end, color='r', weight=edge_len, thickness=5)


print('optimal cycle center is {}'.format(final_center))
print('optimal cycle length is {}'.format(len(cycle_vertex_list)))
print('optimal cycle is {}'.format(cycle_vertex_list))

cycle_f.writelines('{}\n'.format((len(cycle_vertex_list))))
for i in cycle_vertex_list:
    cycle_f.write('{} '.format(i))
cycle_f.close()


# Graph Setting
edge_labels = dict([((u, v,), d['weight']) for u, v, d in Graph.edges(data=True)])
pos=nx.get_node_attributes(Graph, 'pos')
edges = Graph.edges()
colors = [Graph[u][v]['color'] for u, v in edges]
thicknesses = [Graph[u][v]['thickness'] for u, v in edges]
nx.draw_networkx_nodes (Graph, pos, node_size=100, alpha=0.7, node_color='skyblue')
nx.draw_networkx_edges (Graph, pos, width=thicknesses, edge_color = colors)
nx.draw_networkx_labels(Graph, pos, font_size=8, font_weight='bold')    # vertex number
nx.draw_networkx_edge_labels(Graph, pos, edge_labels=edge_labels)   #edge length    if want to see clear image, remove this line

plt.tight_layout()
plt.subplots_adjust(left = 0, bottom = 0, right = 1, top = 1, hspace = 0, wspace = 0)
plt.show()

```

![1](https://user-images.githubusercontent.com/23732754/65012667-26530a00-d953-11e9-8198-76a3392c89a8.png)
![2](https://user-images.githubusercontent.com/23732754/65012676-34088f80-d953-11e9-9754-50845c8ccd54.png)
![3](https://user-images.githubusercontent.com/23732754/65012694-42ef4200-d953-11e9-8044-d4f5db742376.png)
